# OsonSavdo — Inventory & POS (Next.js + TypeScript)

OsonSavdo is a small‑retail focused inventory and point‑of‑sale web application. This repo contains the Next.js (App Router) frontend and server actions used for the MVP. The app is designed to work offline for POS terminals, sync sales to a Neon Postgres backend, and scale later with workers and job queues.

## Quick start (developer)

Prerequisites

- Node.js 18+ (18.x or later recommended)
- pnpm (recommended package manager)

Setup

```bash
pnpm install
cp .env.example .env.local
# Edit .env.local with your Neon DATABASE_URL and other secrets
pnpm prisma generate
pnpm dev
```

Open http://localhost:3000 in your browser.

Key commands

- Install: `pnpm install`
- Dev server: `pnpm dev`
- Build: `pnpm build`
- Start (production): `pnpm start`
- Migrate (dev): `pnpm prisma migrate dev --name init`
- Prisma Studio: `pnpm prisma studio`
- Tests: `pnpm test`

## Environment variables

Create `.env.local` from `.env.example`. Typical variables used by this project:

- DATABASE_URL — Neon/Postgres connection string (required)
- NEXTAUTH_SECRET — secret for session encryption (if using NextAuth)
- NEXT_PUBLIC_BASE_URL — public base URL (optional)
- REDIS_URL — optional (if you enable Redis & BullMQ later)

Do not commit secrets to git. Use platform environment configuration in production.

## Architecture & design notes

- Framework: Next.js (App Router) + TypeScript
- ORM: Prisma (Postgres / Neon)
- Offline / POS: PWA + IndexedDB (Dexie.js) + TanStack Query + Fuse.js for local search
- Background jobs: DB‑backed job table for MVP; Redis + BullMQ planned for later iterations

See `instructions/docs/architecture.md` for the high level architecture and choices.

## Development guidance

- Use TypeScript and add types for inputs/outputs.
- Validate API input with `zod`.
- Keep business logic in `src/server/services/*` so it can be reused by API routes or extracted to a service later.
- Follow eslint/prettier rules (semicolons enabled).

## Testing

- Unit tests: Vitest (run `pnpm test`)
- Add small e2e tests for the POS offline sync flow where possible.

## Deployment

- MVP: Frontend + server actions can be deployed on Vercel. If you use Vercel, keep long‑running workers on a separate host (Render/Fly/DigitalOcean) to avoid serverless DB connection limits.
- Production DB: Neon (managed Postgres) — use pooling features.
- When ready, containerize workers and long‑running services (Docker) and run them on a container host.

## Contributing

- Keep PRs small and focused. Add tests for new logic. Avoid committing secrets.

## Resources

- Architecture & features: `instructions/docs/architecture.md`
- Product spec / backlog: `instructions/docs/all-features-ideas.md`
