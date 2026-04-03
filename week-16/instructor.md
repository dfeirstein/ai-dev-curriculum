# Week 16: Project 3 — Polish and Ship (Week 2/2) — Instructor Guide

## Week at a Glance
- Theme: Production-quality finishing. Students add testing, monitoring, CI/CD, and documentation to their independent project. The goal is a portfolio-worthy shipped product, not more features.
- What students ship: Their independent project with tests passing in CI, Sentry error tracking, PostHog analytics, a GitHub Actions pipeline, a polished README, and a deployed production URL.
- Estimated hours: 12-16
- Difficulty: 3

## Pacing Guide
- **Day 1-2:** Testing. Students direct Claude Code to write unit tests for core logic and at least 1 E2E test for the primary user flow. Set up Vitest, get tests passing. Resist the urge to add features -- this is quality week.
- **Day 3-4:** Monitoring and CI/CD. Add Sentry (error tracking), PostHog (3-5 custom events minimum), and a GitHub Actions workflow that runs tests on PRs. Configure branch protection.
- **Day 5:** Documentation and final polish. Write a proper README (what it does, how to run it, tech stack, screenshot). Final deploy. Demo preparation. Ship.

## Getting Students Started
- The hardest part of this week is the mindset shift. Students want to keep adding features. Your job is to hold the line: no new features. Only quality, testing, monitoring, and polish. Features without quality are not shippable.
- Common confusion: students think "polish" means making it prettier. Polish means: does it handle errors gracefully? Does it work on mobile? Are loading states present? Can someone who is not you understand what it does?
- Students should have open: Claude Code CLI, their Project 3 repo, deployed app, Sentry and PostHog accounts (they already have these from Week 13), and their TeamTask Pro repo as a reference for testing and CI/CD setup.

## Check-in Points
- **After guide.md:** Verify the student has a list of what they need to add this week: tests, Sentry, PostHog, CI/CD, README. They should have a prioritized order. If they mention new features, redirect them.
- **Mid-project checkpoint:** By end of Day 3, tests should be passing and at least one monitoring tool should be integrated. If the student is behind, prioritize CI/CD pipeline over monitoring. A project with CI and tests is more portfolio-worthy than one with analytics but no tests.
- **Pre-ship review:** Run through the full checklist: tests pass (`npm test`), CI pipeline runs on a PR, Sentry catches a triggered error, PostHog records events, README explains the project, deployed URL works, no console errors on the main flow. The student should be able to hand the URL to a stranger and have them understand what the app does.

## Common Pitfalls
- **Feature creep.** "I just need to add one more thing." No. The feature list was locked at the end of Week 15. This week is exclusively about making what exists production-quality. Be firm.
- **Running out of time on testing.** Students who have never written tests independently (they did it in Week 12 with guidance) may struggle. If they are stuck after 4 hours, have them write 3 unit tests for their most important utility functions and move on. Some tests are infinitely better than no tests.
- **Skipping monitoring because "my app is small."** Every production app needs error tracking. Period. Even a simple app will have errors in production. Sentry setup takes 30 minutes. It is non-negotiable.
- **README is an afterthought.** Students write a 2-line README at 4:55 PM on Day 5. The README is the first thing anyone sees on GitHub. It should explain the project, show a screenshot, list the tech stack, and describe how to run it locally. Budget at least 1-2 hours for this.
- **CI pipeline is copied from TeamTask Pro without understanding.** Students should adapt their pipeline, not copy-paste. Different projects have different test commands, different env vars, different build steps.

## How to Know the Student Gets It
- The student prioritizes quality over quantity without being told.
- The student runs their test suite frequently and fixes failures immediately.
- The student's README makes the project understandable to someone who has never seen it.
- The student is proud of the project's quality, not just its features.
- **Red flags:** Student spent Day 1-3 adding features and is now cramming tests on Day 5. Student's CI pipeline has never run successfully. Student's README says "TODO" anywhere. Student's deployed app has visible errors.

## Support Strategies
- At the start of the week, have the student write a physical checklist: tests, Sentry, PostHog, CI/CD, README, final deploy. Check items off as they go. The visual progress helps prevent the "I'll get to it later" trap.
- If a student finished Week 15 with a rough MVP, it is okay to allow 1 day of feature work on Day 1, but only for features that are critical to the core experience. Negotiate this explicitly: "You get Day 1 for features, but Days 2-5 are quality only."
- For the README, give students a template: project name, one-sentence description, screenshot, tech stack list, setup instructions, what you learned. This removes the blank-page problem.

## Differentiation
- **Moving fast?** Add test coverage reporting, a PostHog funnel analysis, performance monitoring in Sentry, and a CONTRIBUTING.md. Write a blog-style "what I learned" section in the README. Set up automated dependency updates.
- **Struggling?** Cut PostHog (keep Sentry). Cut branch protection (keep CI pipeline). The minimum is: tests exist and pass, errors are tracked, CI runs, README is written, app is deployed.
- **Minimum viable ship:** 3 passing unit tests, Sentry installed and catching errors, a CI workflow that runs tests, a README with a project description and screenshot, deployed to Vercel.
