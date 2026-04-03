# Week 13: TeamTask Pro — Monitoring and Analytics

**Theme:** See what's happening in production.

Your application is live. Users are interacting with it. But what are they actually doing? Are they hitting errors you don't know about? Are they using the features you built, or ignoring them? Where do they get stuck?

This week you'll instrument TeamTask Pro with production monitoring and product analytics. Sentry tells you when things break. PostHog tells you how the product is being used. Together, they give you eyes on your production application.

---

## Learning Objectives

1. Understand why monitoring matters -- you can't fix what you can't see.
2. Understand Sentry deeply -- error tracking, source maps, alerts, and release tracking.
3. Understand PostHog deeply -- event tracking, funnels, feature flags, and session replay.
4. Learn to define a tracking plan before implementing analytics.
5. Understand the difference between monitoring (is it working?) and analytics (how is it being used?).
6. Think critically about privacy -- what to track vs. what's invasive.

## What You Ship

TeamTask Pro with **comprehensive monitoring and analytics** -- Sentry error tracking with source maps, alerts, and release tracking; PostHog event tracking with a defined tracking plan, feature flags, and session replay.

## Time Commitment

~10-12 hours across the week.

## Prerequisites

- **Week 12 completed.** TeamTask Pro has a test suite with unit, integration, and E2E tests.
- Your TeamTask Pro repo on GitHub with Vercel deployment.
- Free accounts on Sentry and PostHog (you may already have these from Week 6).

## This Week's Materials

| File | What It Covers |
|------|---------------|
| [guide.md](guide.md) | Monitoring and analytics concepts -- Sentry, PostHog, tracking plans, and privacy -- read this first |
| [project.md](project.md) | Step-by-step project: instrument TeamTask Pro with monitoring and analytics |
| [resources.md](resources.md) | Links for deeper exploration |

## How to Approach This Week

1. **Read guide.md** thoroughly. Understanding what to monitor and what to track is more important than the mechanics of integration.
2. **Work through project.md** step by step. Start with error monitoring, then add analytics.
3. **Use resources.md** when you want to go deeper on Sentry or PostHog.

Monitoring and analytics are invisible features. Users never see them. But they're the features that tell you whether your product is healthy and whether people are getting value from it. They're how you stop guessing and start knowing.
