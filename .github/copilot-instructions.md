# OsonSavdo Project: GitHub Copilot Instructions

## 1. Project Overview & Core Principles

You are an AI assistant helping a solo developer build **OsonSavdo**, a Point-of-Sale (POS) and retail management web application for small to medium businesses in Uzbekistan.

Your primary goal is to generate code that is **clean, performant, secure, and maintainable**. Adhere strictly to the following principles in all generated code:

- **Single Source of Truth (SSoT):** Ensure data and logic originate from a single, authoritative source.
- **Don't Repeat Yourself (DRY):** Aggressively reuse existing functions, components, and logic. Before writing new code, consider if a similar utility already exists.
- **Separation of Concerns (SoC):** Keep UI, business logic, and data access layers distinct.
- **Security First:** Always assume user input is untrusted. Prioritize server-side validation and authorization.

## 2. Technology Stack

Your code suggestions must be compatible with our specific tech stack. Do not suggest alternatives unless explicitly asked.

- **Framework:** Next.js 14+ (App Router)
- **Language:** TypeScript (Strict Mode)
- **Database:** PostgreSQL (hosted on NeonDB)
- **ORM:** Prisma
- **Styling:** Tailwind CSS
- **UI Components:** Shadcn/ui (using `lucide-react` for icons)
- **Authentication:** NextAuth.js (Auth.js v5) with the Prisma Adapter
- **Offline/PWA:** Workbox, Dexie.js, Fuse.js

## 3. Architectural Patterns & Conventions

### Frontend (React / Next.js App Router)

- **Component Structure:**
  - Use Server Components by default. Only use the `"use client"` directive when interactivity (hooks like `useState`, `useEffect`) is absolutely necessary.
  - Separate logic from presentation. Extract complex or reusable logic into custom hooks (e.g., `useShoppingCart.ts`).
  - Organize components into `/components/ui` (from Shadcn), `/components/shared` (reusable across features), and `/features/[feature-name]/components`.
- **State Management:**
  - **Server State:** This is our default. Do **NOT** use client-side state (`useState`, `Zustand`) to store data that comes from the database. Fetch data in Server Components and pass it down as props.
  - **Mutations (Create/Update/Delete):** Use **Server Actions** for all database mutations. This is the primary way the client will interact with the server.
  - **Global Client State:** For simple, non-server state (e.g., UI toggles, theme), use React Context. Only consider `Zustand` if the state becomes highly complex and frequently updated.
  - **Local Component State:** Use the `useState` hook for state that is local to a single component (e.g., form input values).

### Backend & Data Layer

- **Database Access:** ALL database interactions **must** go through the auto-generated Prisma Client. Never write raw SQL.
- **Server Actions & API Routes:**
  - Use **Server Actions** for all form submissions and data mutations initiated by the user.
  - Use **API Routes** primarily for receiving webhooks from external services (e.g., Payme).
- **Input Validation:** All data coming from the client (in Server Actions or API Routes) **must** be validated on the server using **Zod**. This is a non-negotiable security and data integrity step.

### Styling

- Use **Tailwind CSS** utility classes directly in the `className` prop.
- Do not write separate `.css` or `.module.css` files unless absolutely necessary for a complex, unique component.
- When adding new components, use the **Shadcn/ui CLI**: `npx shadcn-ui@latest add [component]`.
- For conditional classes, always use the `clsx` and `tailwind-merge` utilities via the provided `cn` helper function.

### Authentication

- To get the current user's session on the server (in Server Components, Server Actions, API Routes), use the `auth()` helper function from NextAuth.js.
- Protect routes using Next.js **Middleware** (`middleware.ts`).
- Before performing any mutation, always verify that the user has the necessary permissions to perform that action.

## 4. Security Mandates

- **Never expose secret keys** or environment variables on the client side. Ensure they are only accessed on the server and prefixed with `NEXT_PUBLIC_` only if absolutely necessary.
- **Validate all user input** on the server with Zod before passing it to Prisma or any other service.
- **Authorize every action.** Before updating or deleting a resource, verify that the authenticated user is the owner of that resource (e.g., `where: { id: resourceId, storeId: user.storeId }`).
