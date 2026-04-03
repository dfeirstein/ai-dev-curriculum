# Stack Guide

> What each tool does and when to tell Claude Code to use it.

This is your quick reference. When directing Claude Code, you need to know what to ask for — not how it works internally.

---

## Development Environment

### Ghostty
**What it does:** Your terminal — the text interface where you run commands and talk to Claude Code.
**When to use:** Always open. This is your primary workspace.

### Zed
**What it does:** Code editor — where you review files Claude Code creates or modifies.
**When to use:** When you want to browse the file structure or review changes visually. You rarely type here.

### Claude Code CLI
**What it does:** Your AI coding agent. Describe what you want, it builds it.
**When to use:** For everything — building, debugging, testing, deploying, researching, planning.
**Key modes:**
- Research: "Claude, what are the best practices for [X]?"
- Plan: "Claude, plan the architecture for [X]"
- Execute: "Claude, build [X]"
- Review: "Claude, review this code for issues"

### Git
**What it does:** Tracks every change to your project. Save points you can revert to.
**When to use:** After every meaningful change. Direct Claude Code: "Commit these changes with message [X]"
**Key concepts:** Commits (save points), branches (parallel work), merge (combine work), revert (undo)

### GitHub + GH CLI
**What it does:** Stores your code on the internet. Collaboration, PRs, issues.
**When to use:** Every project lives on GitHub. Direct Claude Code: "Push to GitHub" / "Create a PR"

### Vercel
**What it does:** Deploys your app to a live URL. Auto-deploys when you push to main.
**When to use:** Every project gets deployed to Vercel. Connect once, ships automatically.
**Bonus features:** Preview deployments (every PR gets a URL), custom domains, environment variables.

---

## Frontend Stack

### Next.js (App Router)
**What it does:** Web application framework. Handles routing, server rendering, and app structure.
**When to tell Claude:** "Use Next.js with the App Router. Server components by default, client components only for interactive elements."
**Key concepts:** Pages (routes), Layouts (shared UI), Server Components (fast, data access), Client Components (interactive, "use client")

### TypeScript
**What it does:** Catches bugs before your app runs. Like spell-check for code structure.
**When to tell Claude:** "TypeScript strict mode, always."
**Your role:** Run `npm run typecheck`. If it passes, the code is structurally sound. You don't read the types.

### Tailwind CSS
**What it does:** Styling system using utility classes. Composable, responsive, works great with AI.
**When to tell Claude:** "Style with Tailwind CSS. Mobile-first. Consistent spacing."

### shadcn/ui
**What it does:** Pre-built UI components (buttons, forms, cards, dialogs) you own and can customize.
**When to tell Claude:** "Use shadcn/ui components. Install what's needed."
**Why it's special:** The code lives in YOUR project, not in node_modules. Full control.

### ESLint + Prettier
**What they do:** ESLint catches bad patterns. Prettier keeps formatting consistent.
**When to use:** Run `npm run lint` after Claude makes changes. Quality gate.

---

## Backend Stack

### PostgreSQL
**What it does:** Database — structured storage for your application data.
**When to tell Claude:** "Use PostgreSQL for the database."
**Key concepts:** Tables (types of data), rows (entries), columns (fields), relations (connections between tables)

### Neon
**What it does:** Hosted PostgreSQL — a database in the cloud with a connection string.
**When to use:** Create a database at neon.tech, give Claude the connection string.

### Drizzle ORM
**What it does:** Type-safe database toolkit. Translates between your app and PostgreSQL.
**When to tell Claude:** "Use Drizzle ORM. Define schemas for [describe your data]."
**Key concepts:** Schemas (table definitions in code), migrations (database updates), queries

### Zod
**What it does:** Validates data at runtime. Ensures user input is safe and correct.
**When to tell Claude:** "Validate all inputs with Zod schemas."
**Why it matters:** TypeScript checks code at build time. Zod checks user input at runtime. Both are needed.

### API Routes (Next.js)
**What they do:** Endpoints your frontend calls to read/write data. Live in `app/api/`.
**When to tell Claude:** "Create API routes for CRUD operations on [resource]."
**Key concepts:** GET (read), POST (create), PUT (update), DELETE (remove)

---

## Services & Integrations

### Better Auth
**What it does:** Authentication — user accounts, login, sessions, OAuth.
**When to tell Claude:** "Add Better Auth with email/password and GitHub OAuth."

### Stripe
**What it does:** Payments — subscriptions, billing, invoices.
**When to tell Claude:** "Integrate Stripe with [describe tiers]. Use Checkout, webhooks, and Customer Portal."
**Key concepts:** Checkout (payment page), Webhooks (payment event notifications), Feature gating (paid vs free)

### Resend
**What it does:** Transactional email — welcome emails, notifications, password resets.
**When to tell Claude:** "Set up Resend for transactional email."

### Sentry
**What it does:** Error monitoring — catches production bugs you'd otherwise miss.
**When to tell Claude:** "Add Sentry with source maps and release tracking."

### PostHog
**What it does:** Product analytics — user behavior tracking, feature flags, session replay.
**When to tell Claude:** "Add PostHog. Track [these events]. Set up feature flags for [X]."

---

## Quality & Shipping

### Vitest
**What it does:** Testing framework — automated verification that things work.
**When to tell Claude:** "Write tests for [describe what to test]."
**Key types:** Unit tests (individual functions), integration tests (components together)

### Claude Chrome Tool
**What it does:** Browser automation — Claude controls a real browser to test your app.
**When to tell Claude:** "Use the Chrome tool to test [describe the user journey]."

### GitHub Actions
**What it does:** CI/CD — automated quality checks on every code change.
**When to tell Claude:** "Set up GitHub Actions for lint, type-check, test, and build."
**Result:** Every PR is automatically checked. Merge to main = auto-deploy.

---

## The Full Stack in One Prompt

When starting a new project, you can tell Claude Code:

> "Create a Next.js project with TypeScript strict mode, Tailwind CSS, and shadcn/ui. Use Drizzle ORM with PostgreSQL (Neon) for the database. Validate all inputs with Zod. Set up ESLint and Prettier. Create a CLAUDE.md describing this stack."

That one prompt sets up a production-grade foundation.
