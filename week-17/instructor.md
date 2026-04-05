# Week 17: Advanced AI-Native Patterns — Instructor Guide

## Week at a Glance
- Theme: Leveling up as an AI-native developer. Complex prompting strategies, architecture reviews via Claude Code, and contributing to an open source project OR publishing an npm package. This week pushes students beyond building their own apps into working with unfamiliar codebases.
- What students ship: Either a merged (or submitted) open source contribution OR a published npm package. Plus a documented example of using Claude Code for an architecture review of an unfamiliar codebase.
- Estimated hours: 14-18
- Difficulty: 5

## Pacing Guide
- **Day 1:** Advanced prompting patterns. Students read guide.md and practice techniques: multi-step prompting, constraint-based prompting ("build this without using X"), architecture review prompts ("review this codebase and identify the 3 biggest risks"). Practice on their own projects first.
- **Day 2-3:** Open source contribution path OR npm package path. For open source: find a repo, read it with Claude Code's help, find a "good first issue," submit a PR. For npm: identify a small utility, build it, write tests, publish to npm.
- **Day 4-5:** Complete the contribution or package. Write documentation. Reflect on what was different about working in an unfamiliar codebase (or publishing for strangers to use).

## Getting Students Started
- This is the hardest week in the curriculum. Acknowledge that upfront. Working in someone else's codebase is disorienting. Publishing something for public use is intimidating. Both are essential professional skills.
- Common confusion: students do not know how to find an open source project to contribute to. Give them a strategy: search GitHub for `good-first-issue` labels in repos that use their stack (Next.js, TypeScript, React). Alternatively, look at the tools they already use (shadcn/ui, Drizzle, Vitest) for open issues.
- Students should have open: Claude Code CLI, GitHub for browsing repos, npm for package research, and a fresh terminal ready to clone unfamiliar codebases.

## Check-in Points
- **After guide.md:** Verify the student has chosen a path (open source or npm package) and identified a specific target. For open source: which repo, which issue. For npm: what the package does, who would use it. Vague answers ("I'll find something") mean they need help narrowing down.
- **Mid-project checkpoint:** By end of Day 3, the student should have made meaningful progress. For open source: they should have the repo cloned, understand the contribution guidelines, and have a working draft of their change. For npm: the package should work locally with tests. If they are stuck, help them reframe the problem as a prompt for Claude Code.
- **Pre-ship review:** Open source path: PR is submitted (does not need to be merged -- that is outside their control). npm path: package is published and installable via `npm install package-name`. Both paths: the student has a written reflection on what was hard and what they learned about working with unfamiliar code.

## Common Pitfalls
- **Reading unfamiliar codebases is overwhelming.** Students will open a repo with 500 files and freeze. Teach them to use Claude Code strategically: "Explain the architecture of this codebase in 5 sentences." "Where is the code that handles X?" "What files would I need to change to fix issue #123?" Claude Code turns a haystack into a map.
- **Open source contribution fear.** Students are terrified of submitting a bad PR to a public repo. Normalize this: maintainers expect first-time contributors to need guidance. A polite, well-described PR that follows the contributing guidelines is always welcome, even if it needs revision.
- **Choosing a contribution that is too complex.** A first open source contribution should be small: fix a typo in docs, add a missing test, fix a minor bug with a clear reproduction. Do not attempt a major feature.
- **npm package is trivially simple.** A package that exports a single function with no tests, no types, and no README is not publishable. Set a quality bar: TypeScript types, at least 3 tests, a README with usage examples, proper package.json metadata.
- **Skipping the architecture review exercise.** Students may want to jump straight to the contribution/package. The architecture review practice is important -- it builds the skill of understanding unfamiliar code, which is what makes the rest of the week possible.

## How to Know the Student Gets It
- The student can navigate an unfamiliar codebase using Claude Code as a guide, asking targeted questions rather than trying to read every file.
- The student's prompts to Claude Code are specific and contextual ("In this codebase, the tests use Jest with this config. Write a test for this function following that pattern.").
- The student reads contribution guidelines before submitting a PR.
- The student treats the architecture review as a genuine learning exercise, not a box to check.
- **Red flags:** Student refuses to look at unfamiliar code and only works on their own projects. Student submits a PR without reading the contributing guidelines. Student publishes a package with no README or tests.

## Support Strategies
- For open source anxiety, offer to review their PR before they submit it. Having a second pair of eyes reduces the fear significantly.
- If a student cannot find an open source issue to work on, have pre-selected 3-5 repos with good-first-issue labels in the Next.js/TypeScript ecosystem. Offer these as fallback options.
- For the npm package path, help students identify something they built in TeamTask Pro or Project 3 that could be extracted into a reusable package. A date formatting utility, a validation helper, a React hook -- these are all valid packages.
- If a student is truly stuck on both paths, have them do an in-depth architecture review of 2 open source projects instead, with written analysis. The core skill this week is navigating unfamiliar code.
- The advanced `/powerup` lessons ("Auto mode," "Advanced hook patterns," "SDK headless mode," "Cloud task orchestration") are assigned this week. These represent the most powerful Claude Code capabilities and pair well with the advanced patterns content.

## Differentiation
- **Moving fast?** Submit multiple open source contributions. Publish an npm package AND make an open source contribution. Write a blog post about the experience. Add CI/CD to their npm package with automated publishing on version bump.
- **Struggling?** Reduce the open source contribution to a documentation fix or typo correction -- these are legitimate contributions and still require navigating the repo and PR process. For npm, a single-function utility package with types and tests is sufficient.
- **Minimum viable ship:** A submitted PR to any open source repo (even a docs fix) OR a published npm package that installs and works. Plus a written paragraph about what was hard about working in unfamiliar code.
