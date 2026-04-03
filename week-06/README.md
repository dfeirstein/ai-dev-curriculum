# Week 6: Auth, Payments, and Third-Party Services

**Theme:** Understand integrations well enough to direct Claude Code on production-ready applications.

You've built a full-stack app with a database. But real applications need more: user accounts, error monitoring, analytics, email, and eventually payments. This week you'll learn what each of these services does, why they exist, and how to direct Claude Code to integrate them.

By the end of the week, your bookmark manager will have user accounts (each person sees only their own bookmarks), error monitoring that catches production bugs, analytics that track how the app is used, and a welcome email for new signups.

---

## Learning Objectives

1. Understand what **authentication** is and how to direct Claude Code to integrate Better Auth for user accounts and OAuth.
2. Understand how **Stripe** handles online payments, subscriptions, and webhooks at a conceptual level.
3. Understand what **transactional email** is and how to direct Claude Code to integrate Resend.
4. Understand what **error monitoring** does and how to direct Claude Code to add Sentry.
5. Understand what **product analytics** does and how to direct Claude Code to add PostHog.
6. Understand **Vercel's deeper features** -- preview deployments, custom domains, and environment variables.
7. Integrate multiple third-party services into a working application.

## What You Ship

An **authenticated bookmark manager** with user accounts, error monitoring, product analytics, and welcome emails. Each user has their own bookmarks. Production-grade monitoring is in place. Deployed to Vercel.

## Time Commitment

~12-15 hours across the week.

## Prerequisites

- **Week 5 completed.** You have a working bookmark manager with a Postgres database.
- Your bookmark manager repo on GitHub with Vercel deployment.
- You'll create free accounts on several services during this week: Better Auth (uses your existing database), Sentry, PostHog, and Resend.

## This Week's Materials

| File | What It Covers |
|------|---------------|
| [guide.md](guide.md) | Conceptual overview of auth, payments, and third-party services -- read this first |
| [project.md](project.md) | Step-by-step project: add auth and monitoring to your bookmark manager |
| [resources.md](resources.md) | Links for deeper exploration |

## How to Approach This Week

1. **Read guide.md** thoroughly. You're learning about six different services. Each one is simple on its own, but together they form the production layer of a real application.
2. **Work through project.md** step by step. Each integration builds on the last.
3. **Use resources.md** when you want to understand any service more deeply.

After this week, you'll understand the full anatomy of a production web application: frontend, backend, database, auth, monitoring, analytics, and email. Every SaaS product you use daily is built from these same pieces.
