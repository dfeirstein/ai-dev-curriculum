# Project: TeamTask Pro -- Payments and Subscriptions

**What you're building:** Adding Stripe billing to TeamTask Pro with free and paid tiers, checkout, webhooks, subscription management, and feature gating.

**Time estimate:** 10-12 hours

**What you'll need open:** Ghostty (your terminal), a web browser, the Stripe dashboard (test mode), and the Stripe CLI.

---

## The Brief

TeamTask Pro works. Teams can sign up, create projects, manage tasks, and invite members. But it has no business model. Everyone gets everything for free. This week you'll add the payment infrastructure: pricing tiers with real limits, a checkout flow through Stripe, webhook handling for payment events, and a customer portal for subscription management.

After this week, TeamTask Pro has everything it needs to charge real money. You'll work in test mode, but the infrastructure is production-ready.

---

## Phase 1: Research

Before building, understand the tools:

> "Claude, I need to add Stripe subscriptions to my Next.js SaaS app. The tiers are Free (1 project, 3 members) and Pro ($12/month, unlimited). What's the right approach? Should I use Stripe Checkout or build a custom payment form? How do I handle webhooks in Next.js API routes?"

> "Claude, how should I store subscription data? Should I query Stripe every time I need to check a user's plan, or sync the subscription status to my database via webhooks?"

> "Claude, what happens when a subscription payment fails? How does Stripe handle retries? How should my app behave during the retry period?"

> "Claude, how do I test Stripe webhooks locally? Walk me through the Stripe CLI setup."

### Set Up Stripe

Before writing any code:

1. Create a Stripe account at [stripe.com](https://stripe.com) if you don't have one
2. Make sure you're in **test mode** (toggle in the top-right of the dashboard)
3. Install the Stripe CLI: ask Claude for the install command for your OS
4. Create two products in the Stripe dashboard:
   - **Free** -- $0/month (this is just for tracking, no charge)
   - **Pro** -- $12/month
5. Copy the Price IDs for each product (they start with `price_`)

> "Claude, walk me through creating subscription products and prices in the Stripe dashboard test mode. I need a Free tier and a Pro tier at $12/month."

---

## Phase 2: Plan

> "Claude, plan the Stripe integration for TeamTask Pro. Here's what I need:
>
> **Pricing tiers:**
> - Free: 1 project, 3 team members, basic task management
> - Pro ($12/month): unlimited projects, unlimited members, all features
>
> **Checkout flow:**
> - Upgrade button on billing page and contextual upgrade prompts
> - Stripe Checkout session creation
> - Success and cancel redirect pages
>
> **Webhooks:**
> - Endpoint at /api/webhooks/stripe
> - Handle: checkout.session.completed, invoice.paid, invoice.payment_failed, customer.subscription.updated, customer.subscription.deleted
> - Signature verification, idempotency (skip duplicate events)
>
> **Subscription management:**
> - Billing page showing current plan and status
> - Link to Stripe Customer Portal for plan changes and cancellation
>
> **Feature gating:**
> - Check plan limits before creating projects or adding members
> - Show upgrade prompts when limits are hit
> - Enforce in both UI and API
>
> **Database changes:**
> - Add subscription fields to organization: stripeCustomerId, stripeSubscriptionId, stripePriceId, subscriptionStatus, currentPeriodEnd
>
> Plan the implementation order, database changes, API routes, and UI updates."

Review the plan. Key questions:

- How does the app know which tier an organization is on?
- What happens immediately after checkout completes? (Both the redirect and the webhook)
- What happens when a payment fails?
- What happens when a Pro org cancels? Do they keep access until the period ends?
- Are limits enforced in the API, not just the UI?

---

## Phase 3: Execute

### Step 1: Environment Setup

> "Claude, add Stripe environment variables: STRIPE_SECRET_KEY, STRIPE_PUBLISHABLE_KEY, STRIPE_WEBHOOK_SECRET, STRIPE_PRO_PRICE_ID. Update .env.local and .env.example. Install the stripe npm package."

### Step 2: Database Changes

> "Claude, add subscription fields to the organizations table: stripeCustomerId (string, nullable), stripeSubscriptionId (string, nullable), stripePriceId (string, nullable), subscriptionStatus (enum: free/trialing/active/past_due/canceled/unpaid, default 'free'), currentPeriodEnd (timestamp, nullable). Run the migration."

### Step 3: Stripe Checkout

> "Claude, build the checkout flow:
> 1. Create an API route that creates a Stripe Checkout Session for the Pro plan
> 2. The session should include the organization's stripeCustomerId (create a Stripe customer if one doesn't exist)
> 3. Set success_url and cancel_url to redirect back to the billing page
> 4. Include the organizationId in the session metadata so we can link it in the webhook
> 5. Create a billing page at /[org]/settings/billing that shows the current plan and an 'Upgrade to Pro' button"

Test:
- Click "Upgrade to Pro"
- You should be redirected to Stripe's checkout page
- Use test card `4242 4242 4242 4242`, any future expiry, any CVC
- After payment, you should be redirected back to your app

### Step 4: Webhook Handler

Start the Stripe CLI to forward webhooks locally:

```
stripe listen --forward-to localhost:3000/api/webhooks/stripe
```

Copy the webhook signing secret it displays.

> "Claude, build the webhook handler at /api/webhooks/stripe:
> 1. Verify the Stripe signature using the webhook secret
> 2. Parse the event type
> 3. Check for duplicate events (store processed event IDs)
> 4. Handle these events:
>    - checkout.session.completed: look up organization from metadata, save stripeCustomerId, stripeSubscriptionId, stripePriceId, set status to 'active'
>    - invoice.paid: update currentPeriodEnd, confirm status is 'active'
>    - invoice.payment_failed: set status to 'past_due'
>    - customer.subscription.updated: update status and price ID
>    - customer.subscription.deleted: set status to 'canceled', clear subscription fields
> 5. Return 200 for all events (even unhandled ones -- Stripe retries on non-200)
> 6. Log every event received with type and organization"

Test the full flow:
- Complete a checkout with the test card
- Check the Stripe CLI output -- you should see the webhook events
- Check your database -- the organization should now have subscription data
- Refresh the billing page -- it should show "Pro" as the current plan

### Step 5: Customer Portal

> "Claude, add a Stripe Customer Portal link to the billing page. Create an API route that generates a Customer Portal session URL for the current organization's Stripe customer. The portal lets users manage their subscription, update payment method, and cancel. Redirect back to the billing page when done."

Test:
- Click "Manage Subscription" on the billing page
- You should be redirected to Stripe's Customer Portal
- Try canceling the subscription
- After returning to your app, verify the webhook updates the status

### Step 6: Feature Gating

> "Claude, implement feature gating throughout the app:
> 1. Create a utility function that checks the organization's subscription status and returns the current limits (maxProjects, maxMembers)
> 2. Before creating a project: check if the org has reached the Free limit (1 project). If yes, show an upgrade prompt instead of the create form
> 3. Before inviting a member: check if the org has reached the Free limit (3 members). If yes, show an upgrade prompt
> 4. Add these checks to the API routes too -- reject requests that exceed limits with a clear error message
> 5. On the dashboard, show a subtle upgrade banner for Free organizations
> 6. Handle past_due status: show a warning banner prompting the user to update payment"

Test all the gating:
- On a Free org, create 1 project -- should work
- Try to create a second project -- should see upgrade prompt
- Upgrade to Pro via checkout
- Now create more projects -- should work
- Invite more than 3 members -- should work on Pro

### Step 7: Update the Landing Page

> "Claude, update the landing page pricing section with the real tiers: Free (1 project, 3 members, basic tasks) and Pro ($12/month -- unlimited projects, unlimited members, all features). Link the Pro 'Get Started' button to the signup flow."

### Step 8: Billing Status UI

> "Claude, improve the billing page:
> 1. Show the current plan (Free or Pro) with a badge
> 2. If Pro, show the subscription status (active, past_due, canceled) and next billing date
> 3. If past_due, show a prominent warning with a link to update payment method
> 4. If canceled, show when access expires and an option to re-subscribe
> 5. Show 'Manage Subscription' link to Stripe Customer Portal for Pro users
> 6. Show 'Upgrade to Pro' button for Free users"

### Step 9: Quality Gates

> "Claude, run npm run lint and fix any issues."

> "Claude, run npm run typecheck and fix any issues."

> "Claude, run npm run build and make sure it completes without errors."

### Step 10: Deploy

> "Claude, commit all changes, push to GitHub, and deploy to Vercel. Add the Stripe environment variables (STRIPE_SECRET_KEY, STRIPE_PUBLISHABLE_KEY, STRIPE_WEBHOOK_SECRET, STRIPE_PRO_PRICE_ID) to Vercel."

After deploying, set up the production webhook:

> "Claude, walk me through creating a webhook endpoint in the Stripe dashboard that points to my production URL at /api/webhooks/stripe. What events should I subscribe to?"

Test on the live URL:
- Complete a full checkout flow with test card
- Check the Stripe dashboard for the payment
- Verify feature gating works
- Test the Customer Portal

---

## Acceptance Criteria

Your project is complete when all of these are true:

- [ ] Stripe is integrated in test mode
- [ ] Two tiers defined: Free (limited) and Pro ($12/month, unlimited)
- [ ] Checkout flow works (redirects to Stripe, completes payment, returns to app)
- [ ] Webhooks are handled (checkout.session.completed, invoice.paid, invoice.payment_failed, customer.subscription.updated, customer.subscription.deleted)
- [ ] Webhook handler verifies signatures and handles duplicates
- [ ] Customer Portal is accessible for subscription management
- [ ] Feature gating enforced in UI (upgrade prompts at limits)
- [ ] Feature gating enforced in API (requests rejected at limits)
- [ ] Billing page shows current plan, status, and management options
- [ ] Past-due subscriptions show a warning banner
- [ ] Landing page pricing section reflects real tiers
- [ ] `npm run lint` passes
- [ ] `npm run typecheck` passes
- [ ] `npm run build` completes without errors
- [ ] Code is on GitHub and deployed to Vercel

---

## Stretch Goals

**Add Annual Pricing**
> "Claude, add an annual pricing option for Pro: $120/year (save $24). On the billing page, show a toggle between monthly and annual. Create a new Stripe Price for the annual plan. Update the checkout flow to accept either."

**Add a Trial Period**
> "Claude, add a 14-day free trial for Pro. New organizations get Pro features for 14 days before being charged. Update the checkout session to include a trial period. Show trial status and days remaining on the billing page."

**Add Usage-Based Limits**
> "Claude, add a task limit to the Free tier: 50 active tasks per project. Track task counts and show usage progress bars on the dashboard. Warn when approaching the limit."

**Add Proration for Mid-Cycle Upgrades**
> "Claude, if a Free org upgrades to Pro mid-month, Stripe handles proration automatically. Make sure the checkout session has proration enabled and the billing page shows the prorated amount for the first charge."

---

## Troubleshooting

**Checkout redirects but webhook never fires**
> "Claude, I completed checkout but the organization's subscription status didn't update. Check: is the Stripe CLI running and forwarding webhooks? Is the webhook URL correct? Is the signing secret correct? Check the Stripe CLI logs and the server logs."

**Webhook signature verification fails**
> "Claude, the webhook handler returns a signature error. Check: are you using the correct webhook secret (the one from `stripe listen`, not from the Stripe dashboard)? Is the raw request body being passed to the verification function (not a parsed JSON body)?"

**Feature gating not working after upgrade**
> "Claude, I upgraded to Pro but the app still shows Free limits. Check: did the webhook update the database? Is the subscription status 'active'? Is the feature gating function reading the latest data from the database?"

**Duplicate subscription records**
> "Claude, the organization has two subscription entries. The webhook handler is not checking for duplicate events. Add idempotency: store processed event IDs and skip duplicates."

**Customer Portal returns an error**
> "Claude, clicking 'Manage Subscription' throws an error. Check: does the organization have a valid stripeCustomerId? Is the Customer Portal configured in the Stripe dashboard? Is the return URL correct?"

---

## Reflection

You just added a payment system to a SaaS application. Think about what that means: real subscription management, real payment processing, real feature gating based on tiers. This is the same system that every subscription SaaS uses.

Notice the pattern: Stripe handles the hard parts (payment processing, recurring billing, payment UI, compliance). Your app handles the business logic (what each tier gets, what happens when a payment fails, how to prompt users to upgrade). This division of responsibility -- letting specialized services handle specialized problems -- is the core principle of modern software architecture.

Next week, you'll add the communication layer: transactional email and in-app notifications.
