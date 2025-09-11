# OsonSavdo — High Level Architecture

This document describes a recommended, pragmatic architecture for OsonSavdo (MVP -> scale). It focuses on a Next.js full‑stack approach with an offline‑first POS, Postgres (Neon), and a clear migration path to a PERN‑style separation if/when needed.

## Project summary and constraints

- Target users: ~800–1,000 local users (small retail businesses across Uzbekistan). Not all concurrent.
- Primary product needs: fast, reliable POS (one POS per terminal), product catalog, sales, reporting, basic integrations (print‑to‑PDF fallback, later Telegram).
- Network: unstable internet in many locations — offline capability for POS is required.
- Data model: modest catalog size per store; multi‑store may be supported later.
- Persistence: Neon (Postgres) preferred by the team.
- Consistency model: eventual consistency is acceptable.

## Non‑functional requirements

- POS must work reliably when offline for long enough to cover transactions and queue them for sync.
- UI must be snappy: local search and cached product data for instant results.
- Background processing for imports, reports, and retries.
- System must be portable and avoid heavy vendor lock‑in.

## High‑level architecture

Components:

- Client (Next.js PWA) — POS UI, product catalog, local cache, sale queue.
- Server (Next.js server actions / API routes) — business logic, persistence, reconciliation endpoints.
- Database — PostgreSQL (Neon) with Prisma for ORM and migrations.
- Cache & Queue — Redis is recommended for ephemeral cache, locks, and BullMQ job queue/workers at scale, but can be deferred for MVP.
- Workers — separate worker processes/containers for background jobs (imports, sending notifications, processing queued sales). For MVP a simple DB‑backed job table and small polling worker is sufficient.
- DevOps/Hosting — Dockerized services are recommended for portability; Docker is useful when you run workers and Redis, but it is optional for the very first MVP. Recommend Render/Fly/VPS or container host for production workloads. Avoid serverless for long‑running processes that need persistent connections.

Diagram (textual):

Client (PWA)
• Dexie (IndexedDB) cache
• React Query (persisted cache)
• Fuse.js local search
• Service Worker (next-pwa / Workbox) + background sync
↕ sync queue
Server (Next.js) ↔ (optional Redis/BullMQ) ↔ Neon Postgres (Prisma)

## High-level project structure

This section maps the repository layout to responsibilities so developers can quickly find where to add features. It follows the project's conventions (Next.js App Router, TypeScript, Prisma, `src/server/services` for business logic).

Suggested folders and purpose:

- `src/app` — Next.js App Router pages and layouts. Keep route-level Server Components and UI wiring here. Small Server Actions for simple form submissions are allowed, but keep business logic in services.
- `src/components` — Reusable UI components and shadcn wrappers used across pages.
- `src/lib` — Shared utilities (prisma client, date helpers, env helpers, fetch wrappers).
- `src/server/services` — All core business logic lives here (inventory adjustments, sale reconciliation, reports). API routes and Server Actions should import these service functions rather than embedding logic inline.
- `src/server/api` — Next.js API route handlers (optionally under `src/app/api`) that translate HTTP requests into service calls. Keep handlers small and focused on validation (zod) and auth.
- `src/client/offline` — Client-side offline glue: Dexie schemas, sync queue helpers, and service-worker helpers.
- `src/components/pos` — POS-specific components (SearchBar, Cart, CheckoutSummary) and small client-only utilities.
- `prisma` — Prisma schema and migrations.
- `tests` — Unit and integration tests (Vitest + Playwright / MSW for API simulation).

Key starter files to create when scaffolding:

- `src/server/services/salesService.ts` — functions: `createSaleFromSync(payload)`, `applyInventoryAdjustments(tx, lines)`, `markSaleConflict(id, corrections)`.
- `src/app/api/sales/route.ts` — POST handler for `/api/sales/sync` that validates with Zod, calls `salesService.createSaleFromSync`, and returns `ok|conflict` responses.
- `src/client/offline/dexie.ts` — Dexie database schema for products, variants, inventory snapshot, and queuedSales.
- `src/client/offline/sync.ts` — sync worker helpers: `enqueueSale`, `processQueue`, and `handleServerResponse` (maps `corrections[]` to local UI state).
- `src/components/pos/PosPage.tsx` — POS page wiring: loads product catalog into Dexie, provides React Query hooks, and connects the universal search and cart.
- `prisma/schema.prisma` — include `Product`, `Variant`, `Inventory`, `Sale`, `SaleLine`, `Payment`, `User`, `Organization` models as a starting point.

Notes:

- Keep `src/server/services/*` small, focused, and well-tested — this makes extraction to a separate backend straightforward later.
- Avoid putting business logic inside React components or API route handlers; handlers should be adapters that validate, auth, and call services.
- Use zod schemas co-located with service function inputs to keep validation consistent.

## Offline‑first POS design

Core ideas:

- PWA shell cached with service worker (next-pwa or custom Workbox) for fast app load and offline assets.
- Store the full product catalog (or active subset) in IndexedDB using Dexie.js. Keep a stamped version with lastUpdated.
- Use Fuse.js for instant local fuzzy search against the IndexedDB dataset.
- Use TanStack Query (React Query) with cache persistence to coordinate client/server state and provide optimistic updates.
- Sales workflow:
  1. When cashier adds items, the sale is created locally and assigned a temporary id (uuid, timestamp).
  2. Local stock decremented in IndexedDB for immediate feedback.
  3. Sale is queued in IndexedDB and service worker / background sync attempts to POST the sale to server.
  4. Server accepts sale with idempotency key; returns canonical sale id and reconciliation result. Client resolves local temporary id.
  5. Conflicts (e.g., concurrent offline sales causing oversell) are resolved by a clear server+client contract:
     - Server rule: first‑write‑wins. The first POST the server processes that consumes available stock is accepted as-is.
     - Losing-but-still-recorded sales: the server will still persist the sale record when possible but return a `status: 'conflict'` along with `corrections[]` describing what changed (for example reduced quantity, item removed, or a flag requiring manual review).
     - Client behaviour: when a client receives `status: 'conflict'` it must:
       1. Visibly flag the local sale as conflicted (e.g., red banner / warning icon).
       2. Prevent printing a final receipt for that sale until resolved.
       3. Add the sale to a local and server-side "Sales for Review" queue so a manager can reconcile.
       4. Show the human-readable `corrections[]` messages to the cashier so they can inform the customer immediately when appropriate.
     - Rationale: this approach avoids silent failure, gives staff immediate context, and provides an auditable trail for manual reconciliation.
- Printing: Print‑to‑PDF fallback from the browser; later local printer integrations can be added.

## Data model (core entities, high level)

- Product: id, sku, name, attributes, costPrice, salePrice, isVariable
- Variant: id, productId, sku, attributes
- Inventory: variantId, locationId (optional for single-store MVP), quantity — a dedicated inventory table simplifies multi-store and avoids duplicating stock fields on product/variant
- Sale: id, storeId, cashierId, lines[{variantId, qty, price, discount}], payments[], createdAt, status
- PaymentType: id, name, isActive
- Customer: id, name, phone
- Settings: currency, exchangeRate, lowStockThreshold

Keep models normalized and small — Prisma is recommended for schema, types, and migrations. Example Prisma snippet for an explicit inventory model:

```prisma
model Product {
  id        String    @id @default(cuid())
  name      String
  sku       String?   @unique
  variants  Variant[]
}

model Variant {
  id        String      @id @default(cuid())
  product   Product     @relation(fields: [productId], references: [id])
  productId String
  sku       String?     @unique
  // additional attributes
  inventory Inventory[]
}

model Inventory {
  id         String  @id @default(cuid())
  variant    Variant @relation(fields: [variantId], references: [id])
  variantId  String
  locationId String? // add for multi-store later
  quantity   Int

  @@unique([variantId, locationId])
}
```

This makes stock operations explicit and simplifies queries like "stock for variant X at location Y" while keeping Product and Variant models focused on catalog data.

## API & sync contract (sales example)

POST /api/sales/sync

- Request: { idempotencyKey, clientTempId, storeId, cashierId, lines[], totals, payments, createdAt, clientTimestamp }
- Server behavior:
  • Validate idempotencyKey — ensure retry safe.
  • Use transaction: insert sale, decrement stock (per location via `Inventory`), create payment records.
  • Return (success): `{ status: 'ok', canonicalId, serverTimestamp }`
  • Return (conflict):

```json
{
  "status": "conflict",
  "canonicalId": "sale_abc123", // may be present if server persisted a record
  "corrections": [
    {
      "variantId": "v_1",
      "requestedQty": 3,
      "acceptedQty": 1,
      "reason": "Insufficient stock at time of sync"
    }
  ],
  "serverTimestamp": "2025-09-10T12:34:56Z"
}
```

Conflict handling strategy:

- If stock is insufficient for one or more lines, the server should prefer to persist the attempted sale (for audit) while returning `status: 'conflict'` and a `corrections[]` array that describes per-line changes and a human-readable reason.
- If automatic adjustments would produce an unacceptable state (for example, negative inventory or large customer impact), the server can instead mark the sale as `requires_manual_review` and include a `reviewReason` in `corrections[]`.
- Clients must treat `status: 'conflict'` as actionable: flag the sale locally, prevent final receipt printing, and surface the `corrections[]` to the cashier and manager.

## Recommended tech stack and libraries

- Framework & language
  • Next.js (App Router) + TypeScript
  • React (hooks, server components where suitable)

- Client & offline
  • PWA: next-pwa or Workbox
  • Local DB: Dexie.js (IndexedDB wrapper)
  • Client state: TanStack Query (React Query) + persistQueryClient-experimental or local persistence
  • Local search: Fuse.js for fuzzy search
  • Background sync / queue: service worker + background sync API (or IndexedDB + periodic worker sync)

- Server & API
  • Preferred for MVP: Next.js Server Actions and Next.js API routes / REST + zod for explicit validation — this keeps the stack simple and makes it easy to extract an Express backend later.
  • Option (later): tRPC for end‑to‑end types after the API surface stabilizes (nice DX but can be added later).
  • Prisma for DB client and migrations
  • Neon (Postgres) as production DB

- Background jobs & cache
  • MVP note: For the initial MVP, Redis and BullMQ can be deferred. Lightweight alternatives are:
  - Use a DB‑backed job table (Postgres) and a small worker process that polls and processes pending jobs. This avoids adding Redis infra early.
  - Use short‑lived server actions with retry logic for simple tasks and mark heavy jobs (imports/exports) for later.
    • Scale: Add Redis (managed) + BullMQ when import/export, heavy reporting, or high throughput job processing is required.

- Auth & security
  • NextAuth.js (flexible) or Auth adapter that supports sessions/jwt; keep auth provider pluggable
  • Use server side session validation for POS endpoints

- UI & developer tooling
  • Tailwind CSS (already in repo)
  •
- Observability & monitoring
  • Sentry for errors, Prometheus/Grafana or hosted solutions for metrics

## Deployment & hosting recommendations

- Database: Neon (you chose). Use connection pooling features recommended by Neon.
- App & workers: Docker containers deployed on Render / Fly / DigitalOcean App Platform or your own VPS. If you use Vercel for Next frontend, keep workers on a separate host to avoid serverless connection limits.
- Redis: managed Redis (Upstash, Redis Cloud) or a small VPS Redis instance.
- Storage: Use S3-compatible for future product images; for MVP use external image URLs.

## Migration path to PERN or microservices

Keep server business logic in small service modules that are imported by Next API routes. When you need a separate backend:

1. Extract API module into an Express/Koa service that imports the same service modules.
2. Use the same Prisma client and DB schema — no data migration needed.
3. Deploy Express as separate service and point the Next.js client to the new API base URL.

## Operational considerations

- Database migrations: use Prisma migrate and maintain a staging environment.
- Connection limits: prefer pooling or Neon features; do not open many short‑lived DB connections from serverless functions.
- Backups: Neon provides backups; configure retention and periodic export.
- Testing: unit tests for core services, e2e tests for POS offline sync flow (simulate network failures).
- Security: encryption at rest for DB, HTTPS, secure env storage, rotate keys.

## Milestones and next steps

1. Create Prisma schema with core entities (Product, Variant, Inventory, Sale, PaymentType, Customer, Settings).
2. Scaffold Next.js PWA shell with Dexie + React Query + Fuse sample and a simple POS page that loads a seeded catalog into IndexedDB.
3. Implement `/api/sales/sync` endpoint with idempotency and a worker consumer to process queued sales. Ensure the endpoint returns `corrections[]` on conflicts and records persisted sale rows for auditing.
4. Add a "Sales for Review" queue and simple admin UI to view and resolve conflicted sales (manager workflow).
5. Add background job workers (BullMQ) when scale requires it, and tests for offline sync, idempotency, and conflict cases.

## Appendix: quick rationale for major choices

- Next.js: fastest path to ship a full‑stack app with good dev DX.
- Neon/Postgres + Prisma: portable, standards based, low lock‑in.
- Dexie + Fuse + PWA: predictable offline UX and instant local search for POS performance.
- Redis + BullMQ: reliable job processing and retry semantics.

---
