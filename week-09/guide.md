# Week 9 Guide: Payments and Subscriptions

Money is the hardest integration to get right. Not technically -- Stripe makes the technical part straightforward. The complexity is conceptual: understanding the lifecycle of a subscription, handling edge cases gracefully, and making sure your app always knows the correct status of every customer.

This guide covers the concepts you need to understand before directing Claude Code to add Stripe to TeamTask Pro.

---

## Part 1: Subscription Business Models

### The Basics

A subscription SaaS charges users on a recurring basis -- usually monthly or annually. The key metrics:

**MRR (Monthly Recurring Revenue).** How much money comes in each month from subscriptions. If you have 100 customers paying $12/month, your MRR is $1,200. This is the most important number for a SaaS business.

**Churn.** The rate at which customers cancel. If you start the month with 100 customers and 5 cancel, your monthly churn rate is 5%. Low churn means people find your product valuable enough to keep paying.

**ARPU (Average Revenue Per User).** MRR divided by number of paying customers. Helps you understand how much each customer is worth.

You don't need to build analytics dashboards for these metrics right now. Stripe's dashboard shows them. But understanding these concepts shapes how you think about your pricing and features.

### Pricing Tiers

Most SaaS products offer multiple tiers:

**Free tier.** Attracts users with no commitment. Limited features or usage. The goal is to get people using the product so they eventually upgrade. For TeamTask Pro: 1 project, 3 team members, basic task management.

**Pro tier.** The main paid plan. Removes the limits and adds premium features. For TeamTask Pro: unlimited projects and members, priority support, advanced features.

The tier boundary is a critical design decision. Make the free tier useful enough that people actually use it, but limited enough that growing teams need to upgrade.

---

## Part 2: The Stripe Integration Mental Model

Stripe handles the entire payment flow. You don't touch credit card numbers. You don't build checkout forms. You don't manage recurring billing. Stripe does all of that. Your job is to integrate three things:

### Checkout

Stripe Checkout is a hosted payment page. When a user clicks "Upgrade to Pro," you create a Checkout Session (a request to Stripe saying "charge this amount for this subscription") and redirect the user to Stripe's page. They enter their payment details on Stripe's secure page, and Stripe redirects them back to your app when they're done.

The flow:
1. User clicks "Upgrade to Pro" in your app
2. Your app creates a Checkout Session via the Stripe API
3. User is redirected to Stripe's checkout page
4. User enters payment info and confirms
5. Stripe redirects back to your app's success page
6. Stripe sends a webhook to confirm the payment

### Webhooks

This is the most important concept to understand.

A webhook is Stripe calling your app to tell you something happened. It's the reverse of a normal API call -- instead of your app calling Stripe, Stripe calls your app.

Why? Because payment processing is asynchronous. When a user completes checkout, the payment might still be processing. When a subscription renews monthly, your user isn't even on your site. When a payment fails after retries, there's nobody to redirect.

Webhooks solve this. Stripe sends a POST request to a URL you specify (like `https://yourapp.com/api/webhooks/stripe`) with a JSON payload describing what happened: `checkout.session.completed`, `invoice.paid`, `customer.subscription.deleted`, etc.

Your app receives the webhook, verifies it's really from Stripe (using a signing secret), and updates your database accordingly.

### Feature Gating

After you know whether a customer is on the Free or Pro tier, you need to enforce it throughout your app. This is feature gating.

**UI gating:** Don't show features the user can't access. If a Free user can't create more than 1 project, show a message with an upgrade prompt instead of the "Create Project" button after they've used their one project.

**API gating:** Even if someone bypasses the UI, the API must enforce limits. If a Free user sends a request to create a second project, the API rejects it.

**Graceful downgrades:** If a Pro user cancels and drops to Free, their existing data shouldn't be deleted. They just can't create new projects beyond the Free limit until they re-upgrade.

### When Directing Claude Code

> "Claude, the Stripe integration has three parts: Checkout (redirect to Stripe for payment), Webhooks (Stripe tells us about payment events), and Feature Gating (enforce tier limits in UI and API). Keep these three concerns clearly separated."

---

## Part 3: The Subscription Lifecycle

A subscription isn't just "active" or "not active." It goes through stages:

**Trial.** Optional. The user gets Pro features for a limited time before being charged. If they don't cancel before the trial ends, they're charged automatically.

**Active.** The subscription is paid and current. The user has access to all their tier's features.

**Past due.** A payment failed (expired card, insufficient funds). Stripe automatically retries the payment on a schedule. During this period, you can choose to keep features active (grace period) or restrict access.

**Canceled.** The user explicitly canceled. Typically, they retain access until the end of their current billing period. After that, they drop to the Free tier.

**Unpaid.** All retry attempts failed. The subscription is effectively dead. The user loses Pro features.

Your app needs to handle each of these states. The user's experience should be different for each one -- a past-due user might see a warning banner prompting them to update their payment method.

### When Directing Claude Code

> "Claude, track subscription status in the database: trialing, active, past_due, canceled, unpaid. Update the status via webhooks. Show appropriate UI for each status -- active users see Pro features, past-due users see a payment warning banner, canceled users see an upgrade prompt after their period ends."

---

## Part 4: Webhooks Deep Dive

Webhooks deserve extra attention because they're where most payment bugs live.

### Why Idempotency Matters

Stripe may send the same webhook more than once. Network issues, retries, or Stripe's own reliability mechanisms can result in duplicate deliveries. If your webhook handler runs twice for the same event, it should produce the same result both times.

Example of what goes wrong without idempotency: Stripe sends `checkout.session.completed` twice. Your handler creates two subscription records in your database. Now the user appears to have two subscriptions.

The fix: store the Stripe event ID. Before processing a webhook, check if you've already processed that event ID. If yes, skip it.

### Webhook Verification

Anyone could send a POST request to your webhook URL pretending to be Stripe. To prevent this, Stripe signs every webhook with a secret. Your app verifies the signature before processing. If the signature doesn't match, reject the request.

This is a security requirement, not an optional feature.

### The Events That Matter

Stripe sends dozens of event types. For a subscription SaaS, the critical ones are:

- `checkout.session.completed` -- A user just subscribed. Create their subscription record.
- `invoice.paid` -- A recurring payment succeeded. Keep their subscription active.
- `invoice.payment_failed` -- A payment failed. Mark the subscription as past due.
- `customer.subscription.updated` -- The subscription changed (upgrade, downgrade, or status change).
- `customer.subscription.deleted` -- The subscription was canceled and has ended.

### When Directing Claude Code

> "Claude, the webhook handler must: verify the Stripe signature, check for duplicate events using the event ID, and handle these events: checkout.session.completed, invoice.paid, invoice.payment_failed, customer.subscription.updated, customer.subscription.deleted. Log every webhook received."

---

## Part 5: Feature Gating in Practice

Feature gating is where the business model meets the user experience. Get it right and it feels natural. Get it wrong and it feels hostile.

### The Free Tier Experience

Free users should feel like they're using a real product, not a demo. They can create an organization, add team members (up to the limit), create a project, and manage tasks. The product works. It's just limited in scope.

When they hit a limit, the experience matters. Don't show a generic error. Show a clear message: "You've reached the Free plan limit of 1 project. Upgrade to Pro for unlimited projects." Make the upgrade path obvious and frictionless.

### The Upgrade Prompt

The best SaaS products surface upgrade prompts at the moment the user wants more. Not in a popup on login. Not buried in settings. Right there, in context, when they try to do something that requires a higher tier.

### The Downgrade Experience

When a Pro user cancels, don't delete their data. Don't break their workflow. Let them continue until the end of their billing period. After that, they keep their data but can't create new resources beyond Free limits. If they want to re-upgrade, make it one click.

### When Directing Claude Code

> "Claude, implement feature gating throughout the app. Free tier: 1 project, 3 team members, basic task management. Pro tier: unlimited projects and members. When a Free user hits a limit, show an upgrade prompt in context (not a generic error). When checking limits, check in both the UI (hide/disable actions) and the API (reject requests). Handle downgrades gracefully -- don't delete data, just prevent new resources beyond limits."

---

## Part 6: Testing Payments

You will never use real credit cards during development. Stripe provides a complete test environment.

### Test Mode

Stripe has two modes: test and live. In test mode, no real money moves. All API calls use test keys (they start with `sk_test_` and `pk_test_`). Test mode has its own dashboard, its own data, and its own webhook events.

### Test Card Numbers

Stripe provides specific card numbers for testing:
- `4242 4242 4242 4242` -- Always succeeds
- `4000 0000 0000 0002` -- Always declines
- `4000 0000 0000 3220` -- Requires 3D Secure authentication

You'll use these to test the full payment flow without any real money.

### Stripe CLI for Webhooks

In production, Stripe sends webhooks to your live URL. In development, your app is running on localhost -- Stripe can't reach it. The Stripe CLI solves this by forwarding webhooks to your local machine.

You run `stripe listen --forward-to localhost:3000/api/webhooks/stripe` and the CLI creates a tunnel. When Stripe sends a webhook, the CLI forwards it to your local app.

### When Directing Claude Code

> "Claude, set up the project so all Stripe integration uses test mode keys. Add instructions in the README for running `stripe listen` for local webhook testing. Use the test card 4242 4242 4242 4242 for success scenarios."

---

## What's Next

Head to [project.md](project.md) to add Stripe billing to TeamTask Pro.
