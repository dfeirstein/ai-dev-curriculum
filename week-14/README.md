# Week 14: TeamTask Pro — CI/CD and Production Readiness

**Theme:** Automate the shipping pipeline. Make it production-grade.

You've been deploying TeamTask Pro by running quality gates manually and pushing to GitHub. That works when you're the only developer. But it's fragile -- you might forget to run tests, skip the type checker, or push broken code because you were in a hurry.

This week you automate everything. Every push runs lint, type-check, test, and build automatically. Every pull request gets a preview deployment. Main branch requires passing CI and a code review. Performance is optimized. TeamTask Pro goes from "deployed" to "production-ready."

---

## Learning Objectives

1. Understand CI/CD -- Continuous Integration and Continuous Deployment -- and why automation matters.
2. Understand GitHub Actions -- automated workflows that run on every push or PR.
3. Understand the pipeline: push, lint, type-check, test, build, deploy.
4. Understand preview deployments -- every PR gets a live URL for testing.
5. Understand branch protection -- enforcing quality before merging.
6. Understand environment management -- development, staging, production.
7. Understand web performance basics -- Lighthouse, Core Web Vitals, image optimization, caching.
8. Understand secrets management -- keeping API keys safe in automated pipelines.

## What You Ship

A **production-ready TeamTask Pro** with automated CI/CD, preview deployments, branch protection, performance optimization, and a /ship skill that handles the full deploy workflow.

## Time Commitment

~12-15 hours across the week.

## Prerequisites

- **Week 13 completed.** TeamTask Pro has monitoring (Sentry) and analytics (PostHog).
- Your TeamTask Pro repo on GitHub with Vercel deployment.

## This Week's Materials

| File | What It Covers |
|------|---------------|
| [guide.md](guide.md) | CI/CD concepts, GitHub Actions, performance, and production readiness -- read this first |
| [project.md](project.md) | Step-by-step project: automate the pipeline and optimize for production |
| [resources.md](resources.md) | Links for deeper exploration |

## How to Approach This Week

1. **Read guide.md** thoroughly. CI/CD is one of those things that seems optional until you've experienced the alternative. Understand why automation matters.
2. **Work through project.md** step by step. Start with CI, then preview deployments, then branch protection, then performance.
3. **Use resources.md** when you want to go deeper on GitHub Actions or performance optimization.

After this week, TeamTask Pro is complete. Not "tutorial complete" -- production complete. Automated quality gates, preview deployments, performance optimization, monitoring, and analytics. This is the standard that real production applications are held to. And you built it by directing an AI.
