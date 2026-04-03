# Week 6 Resources

Reference material for authentication, payments, and third-party services. Focus on understanding what each service does -- not implementation details.

---

## Better Auth

- [Better Auth Documentation](https://www.better-auth.com/docs/introduction) -- Official getting started guide. Focus on the concepts section to understand the auth model.
- [Better Auth with Next.js](https://www.better-auth.com/docs/integrations/next-js) -- Specific integration guide for Next.js projects.
- [Better Auth with Drizzle](https://www.better-auth.com/docs/adapters/drizzle) -- How Better Auth works with Drizzle ORM for database storage.

## Stripe (Conceptual)

- [Stripe -- How It Works](https://stripe.com/docs/payments) -- High-level overview of how Stripe processes payments. Good conceptual starting point.
- [Stripe Checkout](https://stripe.com/docs/payments/checkout) -- How Stripe's hosted checkout page works. Focus on the flow, not the code.
- [Stripe Billing / Subscriptions](https://stripe.com/docs/billing/subscriptions/overview) -- How recurring subscriptions work. Conceptual overview of tiers, billing cycles, and webhooks.
- [Stripe Webhooks](https://stripe.com/docs/webhooks) -- How Stripe notifies your app about payment events. Understanding the webhook concept is important.
- [Stripe Testing](https://stripe.com/docs/testing) -- Test card numbers and how to develop without real money.

## Resend

- [Resend Documentation](https://resend.com/docs/introduction) -- Getting started guide. Simple API, straightforward setup.
- [Resend with Next.js](https://resend.com/docs/send-with-nextjs) -- Specific guide for sending email from Next.js applications.
- [React Email](https://react.email/) -- Companion project for building email templates with React components. Browse the examples for design inspiration.

## Sentry

- [Sentry for Next.js](https://docs.sentry.io/platforms/javascript/guides/nextjs/) -- The specific integration guide for Next.js. Focus on the "Getting Started" section.
- [Sentry Concepts](https://docs.sentry.io/product/) -- Overview of what Sentry captures and how to use the dashboard.

## PostHog

- [PostHog Documentation](https://posthog.com/docs) -- Getting started guide. Browse the product analytics section for the conceptual overview.
- [PostHog for Next.js](https://posthog.com/docs/libraries/next-js) -- Specific integration guide.
- [PostHog Feature Flags](https://posthog.com/docs/feature-flags) -- How feature flags work. Useful for the stretch goals and future projects.
- [PostHog Session Replay](https://posthog.com/docs/session-replay) -- How session replay works. Powerful for understanding user behavior.

## Vercel (Advanced)

- [Vercel Preview Deployments](https://vercel.com/docs/deployments/preview-deployments) -- How every PR gets its own deployment for testing.
- [Vercel Custom Domains](https://vercel.com/docs/projects/domains) -- How to use your own domain name instead of .vercel.app.
- [Vercel Environment Variables](https://vercel.com/docs/projects/environment-variables) -- Managing secrets for production deployment.
- [Vercel Analytics](https://vercel.com/docs/analytics) -- Built-in performance monitoring.

## OAuth (Conceptual)

- [OAuth 2.0 Simplified](https://aaronparecki.com/oauth-2-simplified/) -- The clearest explanation of how OAuth works at a conceptual level. Worth reading to understand the "Login with GitHub" flow.
- [GitHub OAuth Apps](https://docs.github.com/en/apps/oauth-apps/building-oauth-apps/creating-an-oauth-app) -- How to create an OAuth app on GitHub for the "Login with GitHub" feature.
