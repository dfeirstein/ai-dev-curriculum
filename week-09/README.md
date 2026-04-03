# Week 9: TeamTask Pro -- Payments and Subscriptions

**Theme:** Direct Claude Code to add Stripe billing to your SaaS application.

Your SaaS has users, organizations, and features. Now it needs a business model. This week you'll learn how subscription billing works and direct Claude Code to integrate Stripe into TeamTask Pro. By the end, your application will have free and paid tiers, a checkout flow, webhook handling, and feature gating.

This is where your project becomes a real business. Not because you're charging anyone yet -- you'll use Stripe's test mode -- but because the infrastructure for accepting payments and managing subscriptions will be fully in place.

---

## Learning Objectives

1. Understand **subscription business models** conceptually -- MRR, churn, pricing tiers.
2. Understand the **Stripe integration mental model** -- Checkout, Webhooks, Feature Gating.
3. Understand the **subscription lifecycle** -- trial, active, past due, canceled.
4. Understand **webhooks** deeply -- what they are, why idempotency matters, how to handle them.
5. Understand **feature gating** -- how free and paid tiers see different features.
6. Direct Claude Code to build a **complete payment system** using Stripe test mode.

## What You Ship

**TeamTask Pro with Stripe billing** -- Free tier (limited features) and Pro tier (unlimited). Stripe Checkout for subscription signup, webhook handling for payment events, Customer Portal for subscription management, and feature gating throughout the application.

## Time Commitment

~12-15 hours across the week.

## Prerequisites

- **Week 8 completed.** TeamTask Pro foundation is deployed and working.
- A [Stripe account](https://stripe.com) (free to create, you'll work entirely in test mode).
- The [Stripe CLI](https://stripe.com/docs/stripe-cli) installed for webhook testing.

## This Week's Materials

| File | What It Covers |
|------|---------------|
| [guide.md](guide.md) | Conceptual overview of subscriptions, Stripe, webhooks, and feature gating |
| [project.md](project.md) | Step-by-step project: add Stripe billing to TeamTask Pro |
| [resources.md](resources.md) | Links for deeper exploration |

## How to Approach This Week

1. **Read guide.md** thoroughly. Payments are one of the most important (and most error-prone) integrations in a SaaS. Understanding the concepts before building saves significant debugging time.
2. **Set up your Stripe account** before starting the project. Explore the Stripe dashboard in test mode.
3. **Work through project.md** using the research, plan, execute workflow.
4. **Test obsessively.** Use Stripe's test card numbers. Test success, failure, and edge cases.

After this week, TeamTask Pro will have every piece needed to charge real customers. The only step between test mode and production is flipping Stripe to live mode and connecting a bank account.
