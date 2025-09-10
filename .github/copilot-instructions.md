# OsonSavdo — Copilot Instructions & Architectural Guardrails

## 1. Project Overview

- **Project:** OsonSavdo, a small-retail focused inventory and Point-of-Sale (POS) web app.
- **Target Audience:** Small stores in Uzbekistan.
- **Core Requirement:** An **offline-first** POS is the most critical feature. The app must be fast and reliable even with unstable internet.

## 2. Primary Goals for Copilot

- Generate small, focused changes (one file or one feature area at a time).
- Add tests (Vitest) for any new, non-trivial business logic.
- Adhere strictly to the patterns and file locations defined below.

## 3. Tech Stack Summary

- **Full Stack:** Next.js (App Router) + TypeScript
- **Database:** Neon (Postgres) via Prisma ORM
- **Styling:** Tailwind CSS + shadcn/ui
- **Client Offline Stack:** Dexie.js (IndexedDB), TanStack Query, Fuse.js
- **Package Manager:** pnpm

## 4. Project Structure (Key Folders)

- `src/app` — Next.js App Router pages, layouts, and API routes.
- `src/lib` — Shared utilities (Prisma client, date helpers, etc.).
- `src/components` — Reusable React components (shadcn/ui components live in `src/components/ui`).
- `src/server/services` — **All core business logic lives here.** These modules are imported by API routes and Server Actions.
- `prisma` — Prisma schema (`schema.prisma`) and migrations.
- `instructions/docs` — Architecture, PRDs, and other project documents.

## 5. Coding Patterns & Conventions

### Backend

- **API Style:** Use Next.js API Routes for data retrieval (`GET`) and complex mutations. Use Server Actions for simpler form submissions.
- **Business Logic:** **Never** put business logic directly in an API route handler. Place it in a dedicated service module in `src/server/services/*` and call it from the route.
- **Validation:** All external input (API route bodies, Server Action arguments) **must** be validated with **Zod**.
- **Database:** All database access **must** use the Prisma Client. The `prisma/schema.prisma` file is the single source of truth for data models.

### Frontend

- **Data Fetching:** **Always** use **TanStack Query (React Query)** for fetching and caching server data. **Do not use `useEffect` with `fetch` for this purpose.**
- **Mutations:** Use TanStack Query's `useMutation` hook to handle data changes (create, update, delete).
- **Forms:** Use **React Hook Form** with a **Zod** resolver for all user input forms.
- **Styling:** Use **Tailwind CSS** utility classes. Do not write separate `.css` files.

### Offline-First (Client-Side)

- **Local Database:** Use **Dexie.js** to manage the IndexedDB cache for the product catalog and the queue for pending sales.
- **Local Search:** Use **Fuse.js** for client-side fuzzy searching against the data in Dexie.

## 6. Common Commands

- **Install:** `pnpm install`
- **Dev Server:** `pnpm dev`
- **Run Tests:** `pnpm test`
- **Database Migration:** `pnpm prisma migrate dev`

## 7. Resources for Context

- **Architecture:** `instructions/docs/architecture.md`
- **Feature Ideas:** `instructions/docs/all-features-ideas.md`
- **README:** `README.md`
