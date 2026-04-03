# Week 15: Project 3 — Choose Your Own (Week 1/2) — Instructor Guide

## Week at a Glance
- Theme: Independent project selection and initial build. Students choose what to build, scope it to an MVP, and get the core functionality working. Your role shifts from instructor to advisor.
- What students ship: A deployed MVP of their chosen project with core functionality working. Auth, database, and at least one complete user flow.
- Estimated hours: 14-18
- Difficulty: 4

## Pacing Guide
- **Day 1:** Project selection and scoping. Students should finalize their project idea, write a 1-page plan (what it does, who it is for, MVP feature list, tech stack decisions), and get your approval before writing a single prompt. This day is all planning -- no building.
- **Day 2-3:** Scaffold and build core functionality. Direct Claude Code to set up the project (Next.js, database schema, auth if needed), then build the primary user flow end-to-end. By end of Day 3, the core action of the app should work.
- **Day 4-5:** Build remaining MVP features and deploy. The app should be live on Vercel by end of Day 5 with the core experience working, even if rough around the edges.

## Getting Students Started
- Kick off with excitement: they have proven they can build real software. Now they get to choose what to build. This is the payoff for 14 weeks of skill building.
- Common confusion: analysis paralysis on project choice. If a student cannot decide by mid-Day 1, give them 3 concrete options based on their interests and tell them to pick one in 30 minutes. An okay idea executed well beats a perfect idea never started.
- Students should have open: Claude Code CLI, a blank repo (or use `create-next-app`), their 1-page plan document, and a browser for research. They should reference their TeamTask Pro repo as a pattern library for auth, database, API routes, etc.

## Check-in Points
- **After guide.md:** Verify the student has a specific project idea and a written MVP scope. The scope should be 3-5 features, not 15. Push back hard on anything that sounds like "and then it will also..." -- that is scope creep. Ask: "If you only ship 2 of these features, which 2?"
- **Mid-project checkpoint:** By end of Day 3, the student should have a working app that does one thing. Not a beautiful app. Not a complete app. An app where the core user action works. If they do not have this, something is wrong -- they either scoped too big or got stuck on setup. Intervene immediately.
- **Pre-ship review:** The app is deployed. The core flow works end-to-end (a real user could do the main thing). There are no console errors on the critical path. It does not need to be polished -- that is Week 16.

## Common Pitfalls
- **Scoping too big.** This is the number one issue. Students who just built a full SaaS app think they can build another one in 2 weeks. Remind them: TeamTask Pro took 7 weeks. This project gets 2. Cut the scope in half, then cut it in half again.
- **Analysis paralysis.** Students freeze trying to pick the "perfect" project. Set a hard deadline: project must be chosen by end of Day 1. If they cannot choose, you choose for them. The specific project matters far less than the act of building independently.
- **Copying TeamTask Pro with a different name.** Some students will propose "like TeamTask but for X." Push them to try something with at least one meaningfully different technical challenge -- a different data model, a public-facing component, real-time features, or an external API integration.
- **Skipping the plan.** Students want to jump straight to prompting Claude Code. Without a plan, they will build in circles. The 1-page plan is mandatory before any code is generated.
- **Not leveraging patterns from TeamTask Pro.** Students sometimes start from zero as if they have never built anything. Remind them they have a working reference implementation. Auth, database setup, API routes, error handling -- they have done all of it before.

## How to Know the Student Gets It
- The student can pitch their project in 2 sentences: what it does and who it is for.
- The student proactively cuts features from their plan rather than adding them.
- The student references TeamTask Pro patterns when directing Claude Code ("set up auth the same way we did in TeamTask Pro").
- The student ships something deployed by end of week, even if it is rough.
- **Red flags:** Student is still choosing a project on Day 2. Student's plan has 12 features. Student has a local app that has never been deployed. Student is spending more time on design than functionality.

## Support Strategies
- Your role this week is advisor, not instructor. Ask questions more than give answers: "What would happen if you cut that feature?" "What is the riskiest part of this plan?" "Have you tested that flow yet?"
- Do a 10-minute check-in at the start of each day. Ask: "What did you ship yesterday? What are you building today? What is blocking you?" Keep it tight.
- If a student is stuck on a technical problem, do not solve it for them. Instead, help them formulate the right prompt for Claude Code. "How would you describe this problem to Claude Code so it can help you?"
- For students who finish the core MVP early, resist the urge to add features. Instead, have them polish what exists -- better error handling, loading states, edge cases. Week 16 is for additional features and quality.

## Differentiation
- **Moving fast?** Encourage a more technically ambitious project: real-time features with WebSockets, external API integration, a public-facing marketing page alongside the app, or a more complex data model. They can also start on testing and monitoring early.
- **Struggling?** Simplify the project to a single-user tool with no auth (or simple auth). Examples: a personal bookmarks manager, a markdown note-taking app, a habit tracker. The goal is independent building, not technical complexity.
- **Minimum viable ship:** A deployed Next.js app that does one thing end-to-end. It has a database, it stores user data, and the core action works. It does not need auth, payments, or multiple user roles.
