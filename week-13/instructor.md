# Week 13: TeamTask Pro — Monitoring — Instructor Guide

## Week at a Glance
- Theme: Production observability with Sentry (error tracking) and PostHog (product analytics). Students learn to see what is happening in their live app after users interact with it.
- What students ship: Sentry integrated with error boundaries and source maps, PostHog tracking plan implemented with 5+ custom events, at least one PostHog feature flag controlling a UI element. All running in the deployed app.
- Estimated hours: 10-14
- Difficulty: 3

## Pacing Guide
- **Day 1-2:** Students read guide.md. Set up Sentry: install SDK, configure error boundaries, upload source maps, trigger a test error and verify it appears in the Sentry dashboard. Then configure PostHog: install SDK, verify pageview tracking, set up user identification.
- **Day 3-4:** Build and implement a tracking plan. Students decide which 5-8 events matter most (task created, task completed, member invited, CSV exported, etc.), direct Claude Code to instrument them, and verify events appear in PostHog. Then set up a feature flag in PostHog and wire it to a UI element.
- **Day 5:** Verify everything works end-to-end in production. Create a simple PostHog dashboard with key metrics. Review Sentry for any real errors. Ship.

## Getting Students Started
- Frame this week as "giving yourself superpowers." Right now, the student has no idea what users do in their app or when things break. After this week, they will.
- Common confusion: students conflate error tracking and analytics. Clarify early: Sentry tells you when things break. PostHog tells you how people use your product. Different tools, different purposes, both essential.
- Students should have open: Claude Code CLI, the repo, the deployed app, a Sentry account (free tier), and a PostHog account (free tier). Both accounts should be created before starting the build.

## Check-in Points
- **After guide.md:** Verify the student has created accounts on both Sentry and PostHog. They should be able to explain what a tracking plan is and name 3 events they plan to track.
- **Mid-project checkpoint:** By end of Day 3, Sentry should be catching errors (test by throwing one intentionally) and PostHog should be recording at least pageviews. If the student is behind, prioritize Sentry setup over the tracking plan -- error visibility is more critical than analytics.
- **Pre-ship review:** Trigger an error in production and show it appearing in Sentry within 60 seconds. Perform an action in the app and show the corresponding custom event in PostHog. Toggle a feature flag in PostHog and show the UI change. All three must work.

## Common Pitfalls
- **Sentry DSN committed to the repo.** Students may hardcode the Sentry DSN or PostHog API key. These should be in environment variables. The PostHog key is technically public (it is in client-side JS) but the habit of using env vars matters.
- **Source maps not uploading.** Without source maps, Sentry errors show minified code which is useless. This is the most common Sentry setup issue. Have students verify by triggering an error and checking if the stack trace shows readable file names and line numbers.
- **Tracking plan is too vague.** "Track everything" is not a plan. Push students to be specific: event name, when it fires, what properties it includes. A tracking plan should be a table, not a paragraph.
- **Feature flag not actually gating anything.** Students sometimes set up the flag in PostHog but forget to check it in their code. Have them toggle the flag off and verify the UI element disappears.
- **Monitoring only in development.** Students sometimes only test in localhost. Both Sentry and PostHog must be verified in the deployed production app.

## How to Know the Student Gets It
- The student designs a tracking plan before instrumenting events, not after.
- The student can open Sentry, find a recent error, and read the stack trace.
- The student can open PostHog, filter events by type, and describe what users are doing.
- The student sees monitoring as a product feature ("now I can see if the dashboard is actually being used") rather than a chore.
- **Red flags:** Student has Sentry installed but has never looked at the Sentry dashboard. Student tracks 20 events but cannot explain what any of them mean. Student's PostHog shows zero events in production.

## Support Strategies
- Walk through the Sentry and PostHog dashboards together during the first check-in. These UIs can be overwhelming. Show the student where to find recent errors and recent events.
- If SDK installation is failing, check for version conflicts with Next.js. The Sentry Next.js SDK has specific setup requirements (withSentryConfig in next.config.js). PostHog is simpler but must be initialized in a client component or layout.
- If a student finishes early, have them create a PostHog insight (chart) that answers a real question: "How many tasks are created per day?" or "What percentage of users export CSV?"

## Differentiation
- **Moving fast?** Set up Sentry performance monitoring (traces). Create a PostHog funnel analysis (signup to first task created). Implement a second feature flag that controls access to a beta feature. Add Sentry alerts that notify via email when error rates spike.
- **Struggling?** Cut the feature flag. Focus on Sentry setup with source maps + 3 PostHog custom events. Having working error tracking alone is a major win.
- **Minimum viable ship:** Sentry catches errors in production with readable stack traces. PostHog records pageviews. Both dashboards are accessible.
