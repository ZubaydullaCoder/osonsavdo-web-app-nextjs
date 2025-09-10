```instructions
Project: OsonSavdo — Inventory & POS web app (Next.js + TypeScript)

Purpose
- Help Copilot work effectively in this repository by giving context about the app, stack, structure, coding conventions, and common commands. Keep suggestions small, tested, and reversible.

Elevator pitch
- OsonSavdo is a small‑retail focused inventory and POS web app. Target users are small stores in Uzbekistan. Core needs: fast POS (one terminal per cashier), modest product catalogs, offline‑first behaviour, Neon Postgres backend, and a pragmatic path to scale.

Primary goals for Copilot
- Generate small, focused changes (one file or one feature area) unless asked for broader refactors.
- Add tests for new behavior (unit or small e2e) where feasible.
- Avoid adding secrets or external network calls in generated code.

Tech stack summary
- Frontend/Backend: Next.js (App Router) + TypeScript
- ORM: Prisma + Postgres (Neon)
- Client offline: Dexie.js (IndexedDB), TanStack Query, Fuse.js
- Styling: Tailwind CSS
- Job queue (deferred for MVP): Postgres job table -> later Redis + BullMQ
- Package manager: pnpm

Project structure (key folders)
- `src/app` — Next.js App Router pages and layouts
- `src/lib` — shared utilities and wrappers (Prisma client, api helpers)
- `src/server` — server actions / API route handlers and service modules
- `src/workers` — background worker code (may be empty in MVP)
- `prisma` — Prisma schema and migrations
- `docs` / `instructions/docs` — architecture and design docs

Coding & review guidelines
- Use TypeScript for all new code. Add types for inputs/outputs.
- Prefer small, pure functions and separate business logic into `src/server/services/*` so it can be reused or extracted.
- Validate external input with `zod` in API handlers.
- Follow existing linting rules (ESLint + Prettier). Use semicolons.
- Add tests for any non-trivial logic (Vitest). If adding an endpoint, include a unit test for validation and a small integration test where appropriate.
- Avoid altering unrelated files. Keep diffs focused.

Common commands
- Install: `pnpm install`
- Dev: `pnpm dev`
- Build: `pnpm build`
- Migrate (Prisma): `pnpm prisma migrate dev`
- Run tests: `pnpm test`

Resources for Copilot
- architecture: `instructions/docs/architecture.md`
- product/feature spec: `instructions/docs/all-features-ideas.md`
- README: `README.md`

Safety & limitations
- Do not commit secrets or credentials. Use env vars and document them in README.
- If a requested change touches infra (Docker, hosting, DB credentials), add a short plan and ask for confirmation.
- Prefer using small sample data when adding seeds or fixtures.

```
