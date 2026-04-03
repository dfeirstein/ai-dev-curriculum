# Week 9: TeamTask Pro — Payments — Instructor Guide

## Week at a Glance
- Theme and key concepts: Stripe integration for SaaS billing. Students learn how payment flows work: pricing tiers, Stripe Checkout, webhooks, subscription lifecycle, and feature gating based on plan. All work happens in Stripe test mode — no real money.
- What students ship: TeamTask Pro with Free and Pro tiers, Stripe Checkout, webhook handling, subscription management via Stripe Customer Portal, and feature gating (Free tier limits enforced)
- Estimated hours: 10-12
- Difficulty: 4

## Pacing Guide
- **Day 1-2:** Read guide.md. Set up Stripe account (test mode). Create products and prices in the Stripe dashboard. Install Stripe CLI for local webhook testing. Research the subscription flow with Claude. Build the pricing page.
- **Day 3-4:** Implement Stripe Checkout for the upgrade flow. Set up the webhook endpoint to handle `checkout.session.completed`, `customer.subscription.updated`, and `customer.subscription.deleted`. Sync subscription status to the database. Build the customer portal redirect for managing subscriptions.
- **Day 5:** Feature gating — enforce Free tier limits (1 project, 3 members). Upgrade prompts where limits are hit. Test the full flow end-to-end: free signup, hit limit, upgrade, verify limits lifted, cancel via portal, verify limits re-enforced. Deploy.

## Getting Students Started
- Kick off by walking through the user journey: "You sign up for free. You create a project. You try to create a second project and the app says 'Upgrade to Pro for unlimited projects.' You click upgrade, enter a test card number, and now you're on Pro. Three months later you cancel. The app moves you back to free limits." Every technical concept maps to a step in this journey.
- Common confusion: students think they need to build a payment form with credit card fields. They don't. Stripe Checkout is a hosted page that Stripe provides. The student's app redirects to Stripe, Stripe handles the payment, and Stripe redirects back. The app never touches card numbers.
- Students should have open: Ghostty terminal, browser with the app, Stripe dashboard (test mode), and a terminal running `stripe listen --forward-to localhost:3000/api/webhooks/stripe` for local webhook forwarding.

## Check-in Points
- **After guide.md:** Can the student explain the webhook flow? "Stripe sends a POST request to my app when something happens — like a payment succeeds or a subscription is cancelled. My app receives that event and updates the database." If they think the app polls Stripe for status, correct this — webhooks are push, not pull.
- **Mid-project checkpoint:** Products and prices exist in Stripe dashboard. The pricing page renders with real data. Stripe Checkout redirects work — the student can click "Upgrade," see the Stripe Checkout page, enter the test card (4242 4242 4242 4242), and get redirected back. If the checkout flow is not working by mid-week, debug it now. Everything else depends on it.
- **Pre-ship review:** Full lifecycle test: (1) Sign up on free tier. (2) Hit a limit. (3) Upgrade via Checkout. (4) Verify limits are removed. (5) Open Customer Portal and cancel. (6) Verify limits are re-enforced. If any step fails, the integration is incomplete.

## Common Pitfalls
- **Webhook confusion.** This is the hardest concept of the week. Students do not understand why webhooks exist — "Why can't I just update the database after the checkout redirect?" Explain: the redirect is unreliable (user might close the tab). Webhooks are Stripe's guarantee that your app knows what happened. The webhook is the source of truth, not the redirect.
- **Stripe CLI not running during local development.** Student tests the checkout flow, the payment succeeds on Stripe's side, but the app never updates because no one is forwarding webhooks to localhost. Make `stripe listen` part of the startup routine, like `npm run dev`.
- **Subscription lifecycle complexity.** Students handle the happy path (subscribe) but not the unhappy paths (payment fails, subscription lapses, customer disputes). For this course, focus on: created, updated (plan change), and deleted (cancellation). Skip dunning and dispute handling — mention they exist but don't build them.
- **Stripe test mode vs live mode confusion.** If students see "No such product" errors, they may have created products in live mode but are using test mode API keys (or vice versa). All work should use test mode keys (starting with `sk_test_` and `pk_test_`).
- **Feature gating is an afterthought.** Student gets payments working but forgets to actually enforce limits. The Pro upgrade button works, but free users can still create unlimited projects. The feature gate check must happen at the API route level, not just the UI level — otherwise someone can bypass it with a direct API call.

## How to Know the Student Gets It
- They can explain the full payment lifecycle without looking at code: user clicks upgrade, app creates a Checkout session, Stripe handles payment, Stripe sends a webhook, app updates the database, app checks subscription status when enforcing limits.
- They test with the Stripe test card and can find the corresponding events in the Stripe dashboard webhook log. They connect what they see in their app to what they see in Stripe's dashboard.
- Feature gating works at the API level, not just the UI level. They understand that hiding a button is not security — the server must enforce the limit.
- Red flags: Student says "payments work" but has never run `stripe listen`. Subscription status is hardcoded or only updated on the redirect (not via webhook). Free tier users can still access Pro features. Student created products in live mode.

## Support Strategies
- If stuck on webhooks: Have them open two terminals side by side. Terminal 1: `stripe listen --forward-to localhost:3000/api/webhooks/stripe`. Terminal 2: `stripe trigger checkout.session.completed`. They can see the event arrive in Terminal 1 and watch their app process it. This makes the invisible visible.
- If rushing without reviewing: Ask them to cancel a subscription via the Customer Portal and then check: Does the app correctly re-enforce free tier limits? This tests the most commonly missed flow.
- If frustrated: Stripe has excellent documentation and a forgiving test mode. Reassure them that no real money is involved and they can delete and recreate products/prices freely. The worst thing that happens is they need to re-run a test.

## Differentiation
- **Moving fast?** Add annual billing (yearly discount). Add a billing history page showing past invoices from Stripe. Add usage-based metering (track active tasks per month). Add a "trial" flow with 14-day Pro access for new signups.
- **Struggling?** Simplify to just the upgrade flow: free to Pro via Stripe Checkout. Skip the Customer Portal cancellation flow and skip feature gating. Getting the checkout-to-webhook pipeline working is the critical learning.
- **Minimum viable ship:** A pricing page with Free and Pro tiers. Stripe Checkout works in test mode (user can upgrade). At least one webhook event is handled and subscription status is stored in the database. At least one feature is gated by plan (free users cannot exceed a limit). Deployed live.
