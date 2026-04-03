# Week 12: TeamTask Pro — Testing and Quality

**Theme:** Add automated testing. Make it bulletproof.

v1 is shipped. Users can interact with TeamTask Pro. But how do you know it actually works? Right now, the answer is "because I clicked around and it seemed fine." That's not good enough for a real product.

This week you'll learn about automated testing -- writing code that tests your code. Not by hand (you never write code by hand), but by directing Claude Code to create tests that automatically verify your application works correctly. When those tests pass, you have confidence. When they fail, you catch bugs before your users do.

---

## Learning Objectives

1. Understand why automated testing matters -- confidence, not ceremony.
2. Understand the types of tests: unit, integration, and end-to-end, and when each is appropriate.
3. Understand the testing pyramid -- lots of small tests, fewer big tests.
4. Understand what to test and what not to test -- focus on critical paths.
5. Learn to direct Claude Code to write tests by describing behavior, not code.
6. Use the Claude Chrome tool for end-to-end testing of real user journeys.
7. Understand quality pipelines -- lint, type-check, test, build -- all gates must pass.

## What You Ship

TeamTask Pro with a **comprehensive test suite** -- unit tests for utility functions and validation schemas, integration tests for API routes, and end-to-end tests for critical user journeys. A quality pipeline that catches bugs before they reach users.

## Time Commitment

~12-15 hours across the week.

## Prerequisites

- **Week 11 completed.** TeamTask Pro v1 is shipped with dashboard, admin panel, activity log, and data export.
- Your TeamTask Pro repo on GitHub with Vercel deployment.

## This Week's Materials

| File | What It Covers |
|------|---------------|
| [guide.md](guide.md) | Testing concepts, types of tests, what to test, and how to direct Claude Code to write tests -- read this first |
| [project.md](project.md) | Step-by-step project: add a test suite to TeamTask Pro |
| [resources.md](resources.md) | Links for deeper exploration |

## How to Approach This Week

1. **Read guide.md** thoroughly. Testing concepts are more important than testing mechanics. Understand the "why" before the "how."
2. **Work through project.md** step by step. Each phase adds a different layer of testing.
3. **Use resources.md** when you want to understand testing philosophy more deeply.

Testing isn't glamorous. Nobody will see your tests except you (and your future self when something breaks). But a good test suite is the difference between shipping with confidence and shipping with crossed fingers.
