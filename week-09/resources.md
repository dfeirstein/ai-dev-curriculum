# Week 9 Resources

Reference material for subscription billing, Stripe integration, webhooks, and feature gating. Focus on understanding the concepts and the Stripe dashboard.

---

## Stripe Fundamentals

- [Stripe Documentation](https://stripe.com/docs) -- The main Stripe docs. Extremely well-written. Start with the overview.
- [How Stripe Works](https://stripe.com/docs/payments) -- High-level overview of Stripe's payment flow. Good starting point.
- [Stripe Checkout](https://stripe.com/docs/payments/checkout) -- How Stripe's hosted checkout page works. Focus on the subscription checkout flow.
- [Stripe Billing](https://stripe.com/docs/billing) -- The subscriptions system. Overview of how recurring billing works.
- [Stripe Customer Portal](https://stripe.com/docs/customer-management/portal-deep-dive) -- How to let customers manage their own subscriptions.

## Webhooks

- [Stripe Webhooks Overview](https://stripe.com/docs/webhooks) -- What webhooks are and how they work. Essential reading.
- [Webhook Best Practices](https://stripe.com/docs/webhooks/best-practices) -- Idempotency, signature verification, error handling. Read this carefully.
- [Stripe Webhook Events](https://stripe.com/docs/api/events/types) -- Complete list of event types. Focus on the subscription and invoice events.
- [Stripe CLI](https://stripe.com/docs/stripe-cli) -- Installation and usage guide for the CLI tool you'll use to test webhooks locally.

## Testing

- [Stripe Test Mode](https://stripe.com/docs/testing) -- How test mode works, test card numbers, and testing strategies. Bookmark this page.
- [Test Card Numbers](https://stripe.com/docs/testing#cards) -- Specific card numbers for testing success, failure, 3D Secure, and other scenarios.
- [Testing Webhooks Locally](https://stripe.com/docs/webhooks/test) -- How to use the Stripe CLI to forward webhooks to localhost.

## Subscription Business Models

- [Stripe's Guide to SaaS Billing](https://stripe.com/guides/atlas/saas-pricing) -- Comprehensive guide to pricing strategies for SaaS products. Worth reading for business context.
- [The SaaS Metrics That Matter](https://saastr.com/saastr-podcasts/) -- SaaStr's resources on MRR, churn, and other key metrics. Conceptual business knowledge.
- [Pricing Page Examples](https://pricingpages.xyz/) -- Curated collection of real SaaS pricing pages. Browse for design and tier structure inspiration.

## Feature Gating Patterns

- [Feature Flags and Gating](https://posthog.com/docs/feature-flags) -- PostHog's feature flag system. Can be used alongside subscription-based gating for A/B testing.
- [Entitlements in SaaS](https://www.stigg.io/blog/feature-entitlements-the-definitive-guide) -- Conceptual overview of how SaaS products manage what each tier can access.

## Stripe with Next.js

- [Stripe Next.js Examples](https://github.com/stripe/stripe-js/tree/master) -- Official Stripe examples for JavaScript/TypeScript integration.
- [Next.js Commerce](https://github.com/vercel/commerce) -- Vercel's open-source e-commerce template built with Next.js and Stripe. Browse for patterns.
- [Stripe Node.js Library](https://github.com/stripe/stripe-node) -- The official Stripe SDK for Node.js that you'll use in API routes.
