## Title

Marketing Landing Page (Landing Page MVP)

## Feature Summary

A simple, conversion-focused marketing landing page that introduces OsonSavdo to potential customers, communicates core value propositions, and provides a clear call-to-action to sign up or request early access. This page is a low-risk, high-visibility asset that helps marketing collect interest while core product development (POS, offline features, auth) continues.

## Detailed User Stories

- As a visitor, I want to see a clear headline and short description so I can quickly understand what OsonSavdo does and why it matters.
- As a visitor, I want to review 3–4 short feature highlights so I can evaluate whether the product matches my needs.
- As a visitor, I want to see a pricing snapshot so I can immediately understand the basic plan options and expected limits.
- As a visitor, I want a prominent CTA so I can sign up, request early access, or leave my email for updates.
- As a marketing team member, I want an email-capture fallback if the signup system isn't ready so we can collect leads.

## Acceptance Criteria

For "As a visitor, I want to see a clear headline and short description":

- The hero section contains a single-line headline and a 1–2 sentence supporting paragraph.
- An illustrative image or simple visual cue is present (placeholder allowed).
- The hero is responsive and readable on mobile.

For "As a visitor, I want to review 3–4 short feature highlights":

- There are exactly 3 feature tiles summarizing: (1) Fast POS & offline-first reliability, (2) Simple inventory + reporting, (3) Low-friction onboarding for small stores.
- Each tile contains a 1-line title and a single-sentence explanation.

For "As a visitor, I want to see a pricing snapshot":

- A concise 3-column pricing snapshot (Free / Basic / Pro) is shown with one-line limits (e.g., stores, SKUs, users). No billing integration required.

For "As a visitor, I want a prominent CTA":

- A primary CTA button is present in the hero and the page footer.
- If a signup route exists in the app, the CTA links to it. If no signup route is available, the CTA opens an email-capture modal.
- The email-capture modal accepts an email, validates format, shows success state, and stores the email in localStorage or a placeholder file (no production infra required).

For "As a marketing team member, I want an email-capture fallback":

- The email capture stores submitted emails locally in the browser (localStorage) and displays a confirmation message.
- The page includes clear copy about privacy/consent (short note) and a placeholder for analytics consent.

General acceptance:

- The page passes a quick visual check on desktop and mobile layouts.
- No TypeScript or build errors occur when importing the new files (if later wired into app).

## Key UI/UX Considerations

- Layout: Mobile-first, single-column stack on narrow viewports. Desktop shows hero and features side-by-side where space allows.
- Visual hierarchy: Large headline, supportive sub-headline, bold CTA. Feature tiles use icons and short copy for scannability.
- Accessibility: Semantic HTML, keyboard-accessible CTAs, sufficient color contrast for text and buttons.
- Copy tone: Simple, friendly, small-retailer focused. Use examples from the project idea doc (e.g., "offline-first POS for small shops in Uzbekistan").
- CTA behavior: Prefer direct signup when available; otherwise email capture modal must be quick to complete.
- Images: Use lightweight SVGs or placeholders — no heavy media for MVP.

## Out of Scope

- No billing or payment integration in this PRD — only a pricing snapshot for marketing.
- No full signup/auth backend integration; email capture is a temporary fallback.
- No A/B testing, personalization, or multi-language support in the first iteration.
- No analytics beyond a consent placeholder and a recommended snippet location.

## Notes & Next Steps

- Implement as a static Next.js App route (`src/app/landing` or `src/app/page.tsx`) with small reusable components under `src/components/marketing/`.
- Keep the CTA wiring simple: link to `/auth/sign-up` if present; otherwise open `EmailCapture` modal.
- Add a tiny Vitest snapshot test for `Hero` and `FeaturesGrid` after UI components are created.
- Share the draft copy with the product owner for a final wording pass before launch.
