# Week 6 Guide: Auth, Payments, and Third-Party Services

Your bookmark manager works. It stores data, validates inputs, and has a polished UI. But it's missing something fundamental: it doesn't know who's using it. Anyone who visits the URL sees the same bookmarks. There are no user accounts, no login, no privacy.

This week you'll learn about the services that turn a working app into a production-ready product. Each service handles one concern that you absolutely should not build from scratch.

---

## Part 1: Better Auth -- User Accounts

### What Authentication Is

Authentication answers one question: **who are you?**

When you log into any website, you're authenticating -- proving your identity. After that, the site remembers you (a session) so you don't re-enter your password on every page.

### Why Not Build It Yourself

Authentication is a security minefield. Password hashing, session management, token rotation, CSRF protection, brute force prevention, secure cookie handling -- get any of these wrong and your users' accounts are compromised. Security experts have spent decades on these problems.

Better Auth handles all of it. You integrate it; it handles the hard parts.

### What Better Auth Provides

**Email/password signup and login:** The classic. Users create an account with an email and password. Better Auth hashes the password securely, manages the session, and handles password resets.

**OAuth ("Login with GitHub/Google"):** Instead of creating a new account, users can log in with a service they already trust. They click "Login with GitHub," authorize your app, and they're in. No new password to remember.

**Session management:** After a user logs in, Better Auth creates a session -- a secure token that says "this person is authenticated." The session persists across page loads and expires after a configurable time.

**Protected routes:** Some pages should only be visible to logged-in users. Better Auth lets you enforce this -- if someone who isn't logged in tries to visit `/dashboard`, they get redirected to the login page.

### Key Concepts

**Session:** A temporary proof that you're logged in. After authentication, the server creates a session that persists until you log out or it expires. You don't re-enter credentials on every page because the session vouches for you.

**OAuth:** Delegated authentication. Instead of your app handling passwords for GitHub users, GitHub handles it. Your app just asks GitHub: "Is this person who they say they are?" GitHub says yes or no.

**Protected routes:** Pages that require authentication. The pattern is simple -- before rendering the page, check if there's a valid session. If not, redirect to login.

### When Directing Claude Code

> "Add Better Auth with email/password and GitHub OAuth. Protect the /bookmarks route -- redirect to login if not authenticated. Show the user's email in the nav when logged in. Add a logout button."

---

## Part 2: Stripe -- Payments

### How Online Payments Work

At a high level, the flow is:

1. User clicks "Subscribe" in your app
2. They're redirected to Stripe's secure checkout page
3. Stripe collects their payment information (you never see their credit card number)
4. Stripe charges them
5. Stripe notifies your app that the payment succeeded
6. Your app grants access to the paid features

The critical insight: **you never handle credit card numbers.** Stripe's checkout page handles all the sensitive financial data. Your app just knows "this user paid" or "this user didn't pay."

### Key Concepts

**Checkout:** Stripe's hosted payment page. Professional, secure, handles all the payment UI. You redirect users there; Stripe sends them back when they're done.

**Subscriptions:** Recurring payments. You define tiers -- maybe Free and Pro -- each with a monthly or annual price. Stripe handles billing cycles, renewals, and failed payment retries automatically.

**Webhooks:** Stripe's way of telling your app when something happens. "Payment succeeded." "Subscription canceled." "Payment failed." Your app has an API route that listens for these notifications and acts accordingly.

**Customer Portal:** A Stripe-hosted page where users can manage their own subscription -- upgrade, downgrade, cancel, update payment method. You don't build this UI; Stripe provides it.

**Test mode:** All development uses fake credit cards. No real money changes hands. Stripe provides test card numbers that simulate success, failure, and various edge cases.

### Feature Gating

Once you have payment tiers, you need feature gating -- showing different things to different users based on their plan.

- Free users: 50 bookmarks max, basic search
- Pro users: unlimited bookmarks, advanced search, categories, import/export

The logic is simple: check the user's subscription status, then show or hide features accordingly.

### When Directing Claude Code

> "Integrate Stripe with two tiers: Free and Pro ($10/month). Use Stripe Checkout for signups. Set up webhooks for payment-succeeded and subscription-canceled events. Free users are limited to 50 bookmarks. Pro users get unlimited."

---

## Part 3: Resend -- Transactional Email

### What Transactional Email Is

Transactional emails are triggered by actions, not marketing campaigns. Welcome emails when someone signs up. Password reset links. Receipt confirmations. "Your subscription was renewed" notices.

These are emails your app sends automatically in response to something happening.

### Why Resend

Sending email from an application is surprisingly complex. Deliverability (making sure emails actually arrive and don't go to spam), formatting, templates, tracking -- Resend handles all of this with a simple API.

Resend also supports React-based email templates, which means Claude Code can build your emails using the same component approach as your web pages.

### When Directing Claude Code

> "Set up Resend for transactional email. Send a welcome email when a user signs up -- include their name and a link to get started. Use a clean, simple template."

---

## Part 4: Sentry -- Error Monitoring

### The Problem

Users don't report bugs. They encounter an error, get frustrated, and leave. You never know something broke unless you're watching.

In development, you see errors on your screen. In production, errors happen on your users' screens -- and you're not there to see them.

### What Sentry Does

Sentry catches errors that happen in production and alerts you. When something breaks for any user:

- Sentry captures the error with full context (what happened, where it happened, what the user was doing)
- Sentry sends you a notification (email, Slack, etc.)
- Sentry shows you a dashboard with all errors, how often they occur, and how many users are affected

### Source Maps

When your code is deployed, it's been compiled and minified -- compressed into a format that's efficient for browsers but unreadable for humans. Source maps are a mapping file that translates the compressed code back to your original code. With source maps, Sentry can show you exactly which file and line caused the error.

### When Directing Claude Code

> "Add Sentry for error monitoring with source maps. Configure it to capture unhandled errors in both client and server components."

---

## Part 5: PostHog -- Product Analytics

### What It Does

PostHog tracks how users actually use your app. Not just "someone visited the page" but "someone clicked the 'Add Bookmark' button, then searched for 'design', then deleted two bookmarks, then left."

This data answers critical product questions:
- What features do people actually use?
- Where do people get confused and leave?
- Which pages take too long to load?
- Is the new feature I added getting used?

### Key Features

**Event tracking:** Record specific actions. User signed up. User created a bookmark. User searched. User deleted. Each event includes metadata -- what they searched for, how many bookmarks they had, etc.

**Feature flags:** Release new features to a subset of users first. Ship a new search interface to 10% of users, make sure it works, then roll it out to everyone. This is how production teams ship safely.

**Session replay:** Watch a recording of a user's session. See exactly what they saw, where they clicked, where they got stuck. Incredibly valuable for understanding user experience.

### When Directing Claude Code

> "Add PostHog analytics. Track these events: user-signed-up, user-logged-in, bookmark-created, bookmark-deleted, bookmark-searched, bookmark-filtered-by-tag. Include relevant properties with each event."

---

## Part 6: Vercel -- Your Shipping Pipeline

You've been deploying to Vercel since Week 1. Now understand its deeper features.

### Preview Deployments

Every time you create a pull request on GitHub, Vercel automatically builds and deploys that branch to a unique URL. This means you can test changes before they go live. Share the preview URL with someone for feedback. If something's broken, you catch it before it hits production.

### Custom Domains

Instead of `your-app.vercel.app`, use your own domain name. `bookmarks.yourdomain.com`. Vercel handles the DNS configuration and SSL certificates.

### Environment Variables

You've already used these for your database connection string. In production, all sensitive values -- database URLs, API keys for Stripe, Resend, Sentry, PostHog -- go in the Vercel dashboard under Environment Variables.

### Analytics

Vercel has built-in performance monitoring: how fast your pages load, which pages are slowest, where users are located. This complements PostHog's behavioral analytics with technical performance data.

### When Directing Claude Code

> "Set up a custom domain for the bookmark manager on Vercel. Walk me through the DNS configuration."

> "Add all the API keys (Sentry, PostHog, Resend) as environment variables in the Vercel dashboard."

---

## Part 7: The Production Stack

Here's the complete architecture of your application after this week:

- **Next.js** -- application framework (routing, rendering, API routes)
- **TypeScript** -- code quality gate (structural correctness)
- **Tailwind + shadcn/ui** -- design and components
- **Drizzle + Postgres (Neon)** -- data storage and access
- **Zod** -- runtime data validation
- **Better Auth** -- user accounts and sessions
- **Sentry** -- error monitoring
- **PostHog** -- product analytics
- **Resend** -- transactional email
- **Vercel** -- hosting, deployment, and preview environments
- **Stripe** -- payments (stretch goal)

This is not a toy stack. This is the same set of tools that production SaaS companies use. The only difference between your bookmark manager and a venture-backed product is scale and features -- the architecture is identical.

---

## What's Next

Head to [project.md](project.md) to add auth, monitoring, and email to your bookmark manager.
