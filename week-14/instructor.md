# Week 14: TeamTask Pro — CI/CD — Instructor Guide

## Week at a Glance
- Theme: Continuous integration and deployment with GitHub Actions, preview deploys, branch protection, and performance optimization. This is the final production-readiness week for TeamTask Pro.
- What students ship: A CI pipeline that runs tests and linting on every PR, preview deploys for branches, branch protection rules on main, and at least one performance optimization (image optimization, code splitting, or caching). TeamTask Pro should be fully production-ready.
- Estimated hours: 12-16
- Difficulty: 4

## Pacing Guide
- **Day 1-2:** Students read guide.md. Direct Claude Code to create a GitHub Actions workflow that runs linting and tests on every push/PR. Set up preview deploys on Vercel (this is usually a Vercel dashboard toggle + GitHub integration). Verify the pipeline by opening a PR and watching it run.
- **Day 3-4:** Configure branch protection on main (require passing CI, require PR reviews). Then shift to performance: run Lighthouse on the deployed app, identify the worst scores, and direct Claude Code to fix them (image optimization, dynamic imports, proper caching headers, removing unused dependencies).
- **Day 5:** Final production-readiness review. Run through the full flow: create a branch, make a change, open a PR, watch CI run, check preview deploy, merge, verify production deploy. Fix any remaining issues. Ship.

## Getting Students Started
- Frame this as the "professional engineering" week. Everything they have built works, but a professional project has guardrails that prevent breaking things. CI/CD is those guardrails.
- Common confusion: students think CI/CD is complicated DevOps magic. Demystify it -- a GitHub Actions workflow is just a YAML file that says "when someone pushes code, run these commands." That is it.
- Students should have open: Claude Code CLI, the repo, GitHub repo settings page (for branch protection), Vercel dashboard, and a browser with Lighthouse DevTools.

## Check-in Points
- **After guide.md:** Verify the student can describe what happens when they push code. They should be able to trace the flow: push to branch, CI runs tests, preview deploy builds, PR created, review required, merge to main, production deploy.
- **Mid-project checkpoint:** By end of Day 3, the GitHub Actions workflow should run successfully on a PR (green check). Branch protection should be configured. If CI is failing, prioritize fixing it over starting performance work -- a green CI pipeline is this week's most important deliverable.
- **Pre-ship review:** Open a PR and demonstrate: CI runs and passes, preview deploy URL works, main branch is protected (cannot push directly), Lighthouse score is at least 70 on performance. The student should be able to walk through the entire workflow confidently.

## Common Pitfalls
- **CI workflow YAML syntax errors.** YAML is unforgiving with indentation. Students will get frustrated when Actions fail due to a missing space. Encourage them to use the GitHub Actions YAML validator or have Claude Code lint the file before pushing.
- **Tests pass locally but fail in CI.** Usually caused by missing environment variables in the GitHub Actions environment, a different Node version, or database connection issues. Have the student check the Actions logs carefully and set up the necessary secrets in GitHub repo settings.
- **Preview deploys not connecting.** Vercel preview deploys require the GitHub integration to be properly linked. If it is not working, check Vercel project settings and ensure the GitHub repo is connected.
- **Branch protection locks the student out.** Students sometimes configure branch protection rules that they cannot satisfy (e.g., requiring 2 reviewers when they are the only contributor). Guide them to require 0 reviewers for now but keep the CI check requirement.
- **Performance optimization rabbit hole.** Lighthouse suggests dozens of improvements. Students can spend days chasing a perfect score. Set a target (70+ performance) and stop there. Perfection is not the goal this week.

## How to Know the Student Gets It
- The student understands that CI is not optional -- it is what prevents shipping broken code.
- The student can explain what each step of their GitHub Actions workflow does.
- The student creates a branch, makes a change, opens a PR, and waits for CI before merging -- without being told to.
- The student interprets Lighthouse results and makes targeted fixes rather than trying to fix everything.
- **Red flags:** Student merges to main by bypassing branch protection. Student has a CI workflow that always passes because it does not actually run tests. Student cannot explain what their workflow YAML does.

## Support Strategies
- If GitHub Actions is completely new, walk through the workflow file line by line with the student. Make sure they understand triggers (`on: push`), jobs, steps, and how secrets work.
- For CI failures, have the student read the full Actions log output. Resist the urge to debug for them -- reading CI logs is a skill they need.
- If performance optimization feels abstract, have the student run Lighthouse before and after a single change. Seeing a score jump from 55 to 75 after adding image optimization makes it click.
- Advanced `/powerup` lessons "Worktrees," "Code from anywhere," and "Schedule recurring tasks" are valuable this week. They teach production-grade Claude Code workflows that pair naturally with CI/CD concepts.

## Differentiation
- **Moving fast?** Add a CI step that checks test coverage and fails if it drops below a threshold. Set up staging and production environments with different deploy targets. Add a GitHub Action that posts Lighthouse scores as PR comments. Configure Dependabot for automated dependency updates.
- **Struggling?** Cut performance optimization. Focus entirely on a working CI pipeline (lint + test on PR) and branch protection. These two things alone make the project production-grade.
- **Minimum viable ship:** A GitHub Actions workflow that runs `npm test` on PRs and branch protection that requires CI to pass before merging to main.
