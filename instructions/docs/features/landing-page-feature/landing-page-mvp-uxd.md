# Marketing Landing Page — UI/UX Descriptive Design (Landing Page MVP)

## 1. Feature Overview & User Flow

- Feature: A single-scroll, conversion-focused marketing landing page that communicates OsonSavdo’s value to small retailers and drives a clear CTA to sign up or leave an email for early access.

User Flow:

1. Visitor opens the landing page (mobile or desktop).
2. Hero communicates the value proposition immediately (headline + supporting line) with a primary CTA: "Get early access" / "Sign up".
3. Visitor scrolls to Feature tiles (3 concise benefits). Each tile contains an icon, one-line title, and one-sentence explanation.
4. Visitor reviews a short Pricing snapshot (Free / Basic / Pro) showing limits and a secondary CTA on each plan.
5. Visitor reads a compact FAQ or trust line and reaches the Footer with contact/social links.
6. If the visitor clicks CTA:
   - If `/auth/sign-up` exists: navigate there.
   - If not: open Email Capture Dialog to collect email (single field + privacy note).
7. On successful email capture show a confirmation toast and store the email locally (localStorage) until backend is available.

## 2. Page Layout & Structure

- Global layout: a single, responsive column on small screens; a 2-column hero on wide screens (left: text + CTA, right: visual placeholder).
- Sections (top to bottom):
  1. Header (logo left, small nav links right — Sign in if exists).
  2. Hero (headline, short paragraph, primary CTA, small trust badges or customer count, illustrative SVG/image).
  3. FeaturesGrid (3 tiles, evenly spaced, icon + title + one-line copy).
  4. PricingSnapshot (3 cards in a row on desktop, stacked on mobile).
  5. FAQ / Short copy (3–4 collapsible questions or short answers).
  6. Footer (contact, links, privacy/consent placeholder).

Design rules:

- Mobile-first; all text sizes scale down for narrow screens.
- Keep images as lightweight SVG placeholders for performance.
- Use clear visual rhythm and breathing space between sections (Tailwind spacing scale).

## 3. Component Specification (shadcn/ui)

- Header
  - Components: `Avatar` (logo), `Link` (nav), `Button` (Sign in).
- Hero
  - Components: `Heading` (H1 headline), `Paragraph` (support copy), `Button` (primary CTA — variant="default"), `Card` (visual placeholder) or plain `img`/`svg`.
  - CTA placement: primary CTA inside hero; secondary CTA (ghost) near illustration for demos.
- FeaturesGrid
  - Use Tailwind CSS grid layout (e.g., `grid grid-cols-1 sm:grid-cols-3 gap-6`) with `Card` (each tile) and `Icon` (SVG), `Heading` (tile title), `Paragraph` (tile body).
- PricingSnapshot
  - Components: `Card` (plan card), `Table` (optional small list of limits inside Card), `Button` (Plan CTA — variant="secondary"), `Badge` (for recommended plan).
- Email Capture Dialog
  - Components: `Dialog` (modal), inside: `Heading`, `Paragraph`, `Label`, `Input` (type="email"), `Button` (submit), `Button` (close) — use `Dialog` focus trap and `Input` autofocus. Prefer shadcn `form` primitives (`form`, `label`, `input`) for accessible structure when wiring the form.
  - Validation: client-side email regex; show inline error message under `Input`.
- FAQ
  - Components: `Accordion` (if available) or `Card` with show/hide behavior; `Paragraph` for answers.
- Footer
  - Components: `Footer` (semantic container), `Link`, small `Button` for contact.
- Notifications
  - Use `sonner` (shadcn `sonner` package) for toasts; ensure the `Toaster`/provider is mounted in the root layout (for example `src/app/layout.tsx`). Use `sonner`'s success/error methods for feedback and respect analytics consent before sending events.

Notes on variants and properties:

- Primary CTA: `Button` (size="lg", class="w-full sm:w-auto", variant="default").
- Plan CTA: `Button` (variant="outline" on desktop compact, size="sm").
- Dialog: `Dialog` should set `aria-label` and be dismissible by ESC and outside click.

## 4. Key Interactions & Feedback

- Hero CTA click
  - If signup route exists: immediate navigation with preserved utm query params.
  - Else: open `Dialog` with `Input` focused.
- Email capture
  - Validate email format on submit; show inline error for invalid input.
  - On success: store email in `localStorage` under `osonsavdo_landing_emails` and show a success `Toast` message: "Thanks — we’ll email you updates." Disable submit briefly to prevent duplicates.
  - Show subtle analytics event (placeholder) after consent is given; otherwise do not send events.
- Pricing CTA
  - Clicking a plan CTA either pre-fills plan selection in signup or opens email capture if signup missing.
- Accessibility
  - All interactive elements keyboard focusable and visible focus ring.
  - Images have alt text; icons include aria-hidden where decorative.
  - Contrast ratios must meet WCAG AA for text and buttons.
- Loading & error states
  - For any client-side async (e.g., sending to real backend later) show a compact spinner inside the submit `Button` and inline error text on failure.

## 5. Open Questions

- Exact signup route path: is it `/auth/sign-up`, `/auth/register`, or another path? (needed to wire CTA directly).
- Do we have brand assets (logo SVG, primary color) or should I use placeholders and Tailwind default palette?
- Preferred microcopy for the hero headline and CTA (two headline options would help A/B later).
- Where should collected emails ultimately be sent (which backend endpoint or marketing CRM) when ready?
- Any legal/consent wording required for the region (short privacy text under the email field)?

---

If this looks good, I can now scaffold the page and small components in `src/components/marketing/` and `src/app/landing/page.tsx` and add a tiny Vitest snapshot for `Hero` — say "Scaffold landing page now" to proceed.
