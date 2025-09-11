## Landing Page MVP — Technical Specification

Filename: `landing-page-mvp-techspec.md`

### 1. Overview & Architectural Goals

- Short summary: Implement a static marketing landing page for OsonSavdo with a prominent CTA that either navigates to an existing signup route or opens a local email-capture dialog. The captured emails are stored locally in the browser for MVP and optionally persisted using a small API endpoint (see Data Model). Implementation should use Next.js App Router + TypeScript, Tailwind CSS, and shadcn v2.3.0 components.
- Goals:
  - Fast, accessible, mobile-first landing page.
  - Use shadcn v2.3.0 primitives where appropriate (button, card, dialog, input, accordion, sonner).
  - Minimal backend: optional leads API endpoint with Zod validation.
  - Low friction: CTA either links to `/auth/sign-up` or opens email capture.

### 2. Data Model Changes (Prisma) — Optional

This project may not require a database for the earliest landing page. If you want to persist captured emails, add a small `Lead` model in Prisma.

Prisma change (only new model):

```prisma
model Lead {
  id        String   @id @default(cuid())
  email     String   @unique
  source    String?  // e.g., "landing-page-hero" or "pricing-pro"
  createdAt DateTime @default(now())
}
```

Notes:

- This migration is optional for MVP. For earliest demo keep storage in `localStorage`.

### 3. Backend API Endpoints

Only one minimal endpoint is suggested for when backend capture is needed.

- Route: `POST /api/leads`
  - Description: Accepts a single email address and optional source. Validates input with Zod and writes to the database (if available), returns 201 on success.
  - Request Body Schema (Zod):

```ts
import { z } from "zod";

export const LeadCreateSchema = z.object({
  email: z.string().email(),
  source: z.string().optional(),
});

export type LeadCreate = z.infer<typeof LeadCreateSchema>;
```

- Authorization: Public (no auth required) but rate-limit in production.
- Success Response: { success: true, id: string }
- Error Responses:
  - 400: invalid payload
  - 409: duplicate email (if persisted)
  - 500: server error

Implementation notes:

- Use Next.js route handler under `src/app/api/leads/route.ts`.
- Validate request with `LeadCreateSchema` and return structured errors.

### 4. Frontend Component Contracts & Data Flow

Files to implement (suggested):

- `src/app/landing/page.tsx` — top-level page (Server Component) imports `Hero`, `FeaturesGrid`, `PricingSnapshot`, `FAQ`, `Footer`.
- `src/components/marketing/Header.tsx` — Site header
- `src/components/marketing/Hero.tsx` — Hero with primary CTA
- `src/components/marketing/FeaturesGrid.tsx` — 3 feature tiles
- `src/components/marketing/PricingSnapshot.tsx` — 3 plan cards
- `src/components/marketing/EmailCaptureDialog.tsx` — Client Component; handles email capture
- `src/components/marketing/FAQ.tsx` — Accordion-based Q/A
- `src/components/marketing/Footer.tsx` — Footer and privacy text

Component contracts (inputs/outputs):

- Hero
  - Props: { onPrimaryCTA?: () => void; signupPath?: string | null }
  - Behavior: If `signupPath` provided, CTA should navigate using Next `Link`. If not provided, call `onPrimaryCTA` to open dialog.

- EmailCaptureDialog (Client)
  - Props: { open: boolean; onClose: () => void; defaultSource?: string }
  - Behavior:
    - Validate email (client side) and store in localStorage under `osonsavdo_landing_emails` (array of objects { email, source, createdAt }).
    - If a backend is available, POST to `/api/leads` and handle responses (409 duplicate -> show message, 201 success -> add to local store).
    - Show `sonner` toast on success or error.

Data shapes:

LocalStorage key: `osonsavdo_landing_emails`
Value: JSON.stringify(Array<{ email: string; source?: string; createdAt: string }>)

### 5. Implementation Plan (step-by-step)

1. Create files and components listed above.
   - CLI: none required — use editor or run git operations.
2. Add `sonner` package if missing:

```bash
pnpm add sonner
```

3. Implement `EmailCaptureDialog` as a client component using shadcn `dialog`, `input`, and `button` primitives. Use local state and LocalStorage for MVP.
4. Wire hero CTA: read existence of signup route by simple feature flag or config. For now configure `signupPath` in a top-level constant `src/config/site.ts`.
5. Add optional API route: `src/app/api/leads/route.ts` with `LeadCreateSchema` validation and Prisma write (if Prisma is configured).
6. Add `Toaster` provider from `sonner` to `src/app/layout.tsx`.
7. Add minimal Vitest tests: snapshot for `Hero` and a unit test for `EmailCaptureDialog` client logic (validate email and store localStorage).

CLI commands for DB migration (only if using Prisma):

```bash
npx prisma migrate dev --name add_lead_model
```

### 6. Security & Privacy Considerations

- Do not collect more than the required fields. Keep only email and optional source for MVP.
- Add short privacy copy near the email field: "We’ll only use your email to send product updates. You can unsubscribe anytime." (local legal team to confirm).
- Rate-limit the `POST /api/leads` endpoint in production to prevent abuse.
- Sanitize and validate inputs with Zod.
- Respect analytics consent before firing any tracking events.

### 7. Accessibility & Internationalization

- Follow WCAG: keyboard accessibility, visible focus indicators, alt text for images.
- Keep text content in separate files for future translations; for MVP hardcode English/Uzbek-friendly copy.

### 8. Acceptance Criteria & Quality Gates

- Page renders on mobile and desktop (manual visual check).
- Hero CTA either navigates to signup route or opens Dialog.
- Email capture stores data in localStorage and shows a success toast.
- No TypeScript errors; `pnpm build` should succeed.
- Two small tests (Hero snapshot, Email logic unit test) pass.

### 9. Open Questions

- Confirm signup route path (e.g., `/auth/sign-up`).
- Confirm whether to persist leads to DB or keep localStorage for MVP.
- Provide brand color tokens and logo SVG for final UI polish.

### 10. Next Steps

- Confirm open questions above.
- If approved, I can scaffold the page and components now (I will create the files, add `sonner` provider, and add a tiny Vitest test). Reply "Scaffold landing page now" to proceed.
