# Week 18: Speed Build — Instructor Guide

## Week at a Glance
- Theme: Build and ship a complete project in 5 days. This tests everything the student has learned: scoping, planning, building with Claude Code, testing, deploying, and shipping under time pressure.
- What students ship: A complete, deployed, tested application built from scratch in one week. The app should have auth (if applicable), a database, CI, basic tests, error tracking, and a README.
- Estimated hours: 16-20
- Difficulty: 4

## Pacing Guide
- **Day 1:** Ideation and planning in the morning. Scaffold and core data model by afternoon. The project must be scoped to what can realistically be built in 4 remaining days. Deployed to Vercel by end of day, even if it is just a landing page.
- **Day 2-3:** Core feature build. All primary user flows should work by end of Day 3. This is the heads-down building phase. Students should be in a flow state, directing Claude Code through feature after feature.
- **Day 4:** Testing, monitoring, and CI. Vitest tests, Sentry integration, GitHub Actions pipeline. This is not optional -- quality is part of shipping.
- **Day 5:** Polish, README, final deploy, demo. The app should be something the student would show to a hiring manager.

## Getting Students Started
- Set the tone immediately: this is a sprint, not a marathon. Clock starts now. Students who spend Day 1 debating project ideas will not finish. Give them 90 minutes max to choose and plan, then they must start building.
- Common confusion: students scope as if they have 2 weeks. They do not. A speed build project should have 2-3 features, not 8. If it sounds like it would take 2 weeks, it will take 3, and they have 5 days.
- Students should have open: Claude Code CLI, a timer or calendar with daily milestones visible, their TeamTask Pro and Project 3 repos as reference, and a fresh GitHub repo ready to go.

## Check-in Points
- **After guide.md:** By end of Day 1 morning, the student must have: a project idea in one sentence, a list of exactly 3 features for the MVP, and a daily plan. If any of these are missing, stop and help them get there before they start building.
- **Mid-project checkpoint:** End of Day 3 is the critical gate. The core features must work. If they do not, the student needs to cut scope immediately. Have a frank conversation: "What is the one feature that must work? Focus only on that." Cut everything else.
- **Pre-ship review:** End of Day 4: tests pass, Sentry is integrated, CI runs. Day 5 morning: README written, final polish. Day 5 afternoon: deployed, demo-ready. If tests and CI are not done by end of Day 4, they will not happen -- intervene early.

## Common Pitfalls
- **Over-scoping for 5 days.** The single biggest risk. A speed build is a small, complete, polished app -- not a big, incomplete, rough app. Acceptable speed build projects: a link shortener, a poll creator, a simple blog platform, a recipe saver, a bookmark manager. Not acceptable: a full marketplace, a social network, a project management tool.
- **Day 3 panic.** Students hit Day 3, realize they are behind, and either panic-code messy features or freeze up. Prevent this with the Day 3 checkpoint. If they are behind, cut scope ruthlessly. A working app with 1 feature is better than a broken app with 3.
- **Skipping quality on Day 4.** "I'll add tests later" means "I will not add tests." The pacing guide puts quality on Day 4 for a reason -- it is the last full build day. Day 5 is too late to start testing.
- **Shipping unpolished work.** Speed does not mean sloppy. The README should be clear. The UI should not have broken layouts. Error states should not show stack traces. Budget Day 5 morning for these details.
- **Reusing too much from previous projects.** It is fine to reference patterns, but copy-pasting entire features from TeamTask Pro defeats the purpose. The speed build tests whether the student can build independently and quickly, not whether they can duplicate existing work.

## How to Know the Student Gets It
- The student scopes aggressively small and finishes early rather than scoping big and scrambling.
- The student follows the daily milestones without being reminded.
- The student includes testing and monitoring without being told -- it is part of their mental model of "done."
- The student ships something they are proud of on Day 5, not something they are apologizing for.
- **Red flags:** Student is still building features on Day 5. Student has no tests. Student's app is not deployed. Student's README is missing. Student seems burned out or defeated rather than energized.

## Support Strategies
- Check in briefly every morning: "What is your goal for today? What could block you?" Keep it to 5 minutes. The student should be building, not meeting.
- If a student is frozen on Day 1, give them a project idea: "Build a link shortener with custom slugs, click analytics, and user auth. Go." Removing the choice removes the paralysis.
- On Day 3, if a student is behind, sit with them for 15 minutes and help them triage. Write a list of what is done, what is in progress, and what is not started. Cross off anything not started -- that is the new scope.
- Celebrate speed and decisiveness. When a student makes a fast decision ("I'm cutting that feature"), praise it. Speed building is as much about what you do not build as what you build.

## Differentiation
- **Moving fast?** Add a stretch feature on Day 4 afternoon (after tests and CI are done). Write a technical blog post about the build process. Challenge them with a constraint: "Can you add real-time updates using server-sent events?"
- **Struggling?** Reduce to a single-feature app with no auth. A public tool (like a color palette generator or a markdown previewer) that stores nothing in a database is a valid speed build. The student still practices scoping, building, testing, and deploying.
- **Minimum viable ship:** A deployed app that does one thing, has a README, and has at least 1 passing test. Sentry is installed. The student can demo it in 60 seconds.
