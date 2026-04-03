# Project: TeamTask Pro — CI/CD and Production Readiness

**What you're building:** An automated CI/CD pipeline with GitHub Actions, preview deployments, branch protection, performance optimization, and a /ship skill. TeamTask Pro becomes a production-grade application.

**Time estimate:** 8-10 hours

**What you'll need open:** Ghostty (your terminal) and a web browser

---

## The Brief

This is the final week of TeamTask Pro development. You're going from "deployed" to "production-ready" -- which means automated quality enforcement, preview deployments for testing changes, branch protection to prevent accidents, and performance optimization to make the app fast.

When you're done, the entire workflow is automated: push code, CI verifies quality, preview deployment lets you test, merge to main, production deploys automatically. No manual steps. No forgotten checks. No broken production.

---

## Phase 1: Research

Start Claude Code and research the CI/CD approach:

> "Claude, I have a Next.js app (TeamTask Pro) on GitHub deployed to Vercel. I want to set up GitHub Actions CI that runs lint, type-check, test, and build on every PR. I also want preview deployments, branch protection on main, and performance optimization. What's the best approach?"

Follow up:

> "What does a GitHub Actions workflow file look like for a Next.js project? What are the key decisions -- Node version, caching, parallel vs. sequential jobs?"

> "How do I set up GitHub Secrets for the test database URL and other environment variables the CI pipeline needs?"

> "What's the best way to run Lighthouse audits -- in CI or manually?"

---

## Phase 2: Plan

> "Claude, plan the complete CI/CD and production readiness setup for TeamTask Pro. I need: (1) GitHub Actions CI workflow that runs lint, typecheck, test, and build on every push and PR, (2) Vercel preview deployments configured for every PR, (3) branch protection on main requiring passing CI and one review, (4) Lighthouse performance audit and optimization, (5) image optimization, caching, and lazy loading, (6) a /ship skill that handles the branch-PR-merge workflow. Plan the order of implementation and any GitHub/Vercel configuration needed."

Review the plan. Key questions:

- What environment variables does CI need, and how are they provided?
- Should CI jobs run in parallel or sequence?
- What Lighthouse score targets are we aiming for?
- How does the /ship skill handle the full workflow?

---

## Phase 3: Execute

### Step 1: GitHub Actions CI

> "Claude, set up a GitHub Actions CI workflow for TeamTask Pro. Create a .github/workflows/ci.yml file that: (1) triggers on push to main and on all pull requests, (2) runs on Ubuntu latest with Node.js 20, (3) caches node_modules for faster runs, (4) installs dependencies, (5) runs npm run lint, (6) runs npm run typecheck, (7) runs npm run test (unit and integration tests), (8) runs npm run build. Each step should fail the pipeline if it fails. Add a job summary at the end showing which steps passed."

Set up the required secrets:

> "Claude, what GitHub Secrets do I need to add for the CI pipeline to run tests? Walk me through adding them in the GitHub repository settings."

Go to your GitHub repository > Settings > Secrets and variables > Actions, and add the necessary secrets (test database URL, any required API keys for tests).

Test the CI pipeline:

> "Claude, create a new branch, make a small change (add a comment to a file), commit, push the branch, and open a pull request. Let's see if CI runs."

Watch the GitHub Actions tab on your repository. Verify:
- The workflow triggers automatically
- All steps run in order
- All steps pass with green checks
- The PR shows the CI status

### Step 2: Preview Deployments

Vercel preview deployments are likely already working if your repo is connected to Vercel. Verify:

> "Claude, is the PR I just created showing a Vercel preview deployment? Check the PR comments or the Vercel dashboard."

If preview deployments aren't working:

> "Claude, configure Vercel to create preview deployments for every PR on the TeamTask Pro repository. Walk me through the Vercel dashboard settings."

Visit the preview deployment URL from the PR. Verify:
- The preview loads correctly
- It uses the PR's code changes
- It works like the production app (auth, data, etc.)

### Step 3: Branch Protection

> "Claude, walk me through setting up branch protection on the main branch. I want to require: (1) the CI workflow to pass before merging, (2) at least one approval on the PR (I'll self-approve for now), (3) the branch to be up-to-date with main before merging, (4) no force pushes to main."

Go to your GitHub repository > Settings > Branches > Add branch protection rule:
- Branch name pattern: `main`
- Require status checks to pass before merging: enable, select the CI workflow
- Require pull request reviews before merging: enable, require 1 approval
- Require branches to be up-to-date before merging: enable
- Block force pushes: enable

Test it:

> "Claude, try to push directly to main. It should be blocked."

> "Claude, merge the test PR (approve it first if needed). Verify CI passes and the merge succeeds."

### Step 4: Lighthouse Audit

> "Claude, run a Lighthouse audit on the TeamTask Pro production URL. Show me the scores for Performance, Accessibility, Best Practices, and SEO. List every issue that's scoring below 90."

Review the Lighthouse results. Common issues:

> "Claude, based on the Lighthouse audit, fix all the issues that are bringing the score below 90. Prioritize: (1) performance issues first (they affect users most), (2) accessibility issues (they affect whether everyone can use the app), (3) best practices and SEO."

### Step 5: Image Optimization

> "Claude, audit all images in TeamTask Pro. Replace any raw img tags with the Next.js Image component for automatic optimization. Add explicit width and height to prevent layout shift. Convert any large PNG images to WebP format. Add lazy loading for images that are below the fold."

Verify:
- Run Lighthouse again -- image-related scores should improve
- Visually check that images still look correct
- Check that page load feels faster

### Step 6: Caching and Lazy Loading

> "Claude, add caching for expensive database queries in TeamTask Pro. The dashboard aggregation queries (tasks by status, weekly completions, member activity) should be cached for 60 seconds using Next.js unstable_cache or a similar approach. Also implement lazy loading for the dashboard charts -- they should load after the initial page render, not block the page from appearing."

> "Claude, add lazy loading for any heavy components that aren't in the initial viewport. The activity log, the member management table, and the charts should all load on demand."

Verify:
- The dashboard loads faster (the page appears before charts fully render)
- Subsequent dashboard visits within 60 seconds return cached data
- Run Lighthouse again -- performance score should improve

### Step 7: Run Lighthouse Again

> "Claude, run Lighthouse again on the production URL after all optimizations. Compare the scores to the first audit. Show me the before and after."

Target scores:
- Performance: 90+
- Accessibility: 90+
- Best Practices: 90+
- SEO: 90+

If any score is still below 90:

> "Claude, the [category] score is still at [number]. What specific issues are remaining? Fix the highest-impact ones."

### Step 8: The /ship Skill

> "Claude, create a /ship skill in CLAUDE.md that handles the full shipping workflow: (1) Create a new branch with a descriptive name based on the changes, (2) Commit all staged changes, (3) Push the branch, (4) Open a pull request with a clear title and description summarizing the changes, (5) Wait for CI to pass, (6) Show the preview deployment URL, (7) After review, merge the PR. The skill should handle each step and report status along the way."

Test the skill:

> "Claude, make a small improvement to the app (fix a typo, adjust spacing, improve an error message). Then use /ship to ship it."

Verify:
- The branch gets created
- The PR opens with a good description
- CI runs and passes
- The preview deployment is accessible
- The merge succeeds
- Production deploys automatically

### Step 9: Final Quality Check

> "Claude, run the full quality pipeline one final time: lint, typecheck, test, build. Everything must pass."

> "Claude, run a final Lighthouse audit. Confirm all scores are 90+."

> "Claude, commit any remaining changes, push, and deploy."

---

## Acceptance Criteria

Your project is complete when all of these are true:

- [ ] GitHub Actions CI workflow runs lint, typecheck, test, and build on every push and PR
- [ ] CI passes on the main branch
- [ ] Vercel preview deployments are created for every PR
- [ ] Branch protection is enabled on main (require CI + review)
- [ ] Direct pushes to main are blocked
- [ ] Lighthouse Performance score is 90+
- [ ] Lighthouse Accessibility score is 90+
- [ ] Lighthouse Best Practices score is 90+
- [ ] Lighthouse SEO score is 90+
- [ ] Images are optimized with Next.js Image component
- [ ] Dashboard queries use caching
- [ ] Heavy components use lazy loading
- [ ] GitHub Secrets are configured for CI (no secrets in code)
- [ ] A /ship skill handles the full branch-PR-merge workflow
- [ ] All quality gates pass (lint, typecheck, test, build)
- [ ] Code is on GitHub
- [ ] App is deployed and functional on Vercel

---

## Stretch Goals

**Lighthouse CI in GitHub Actions**
> "Claude, add a Lighthouse CI step to the GitHub Actions workflow that runs a Lighthouse audit on the preview deployment URL. Fail the CI pipeline if any Lighthouse score drops below 85. This prevents performance regressions from being merged."

**Staging Environment**
> "Claude, set up a staging environment for TeamTask Pro. Create a 'staging' branch that auto-deploys to a separate Vercel deployment with its own environment variables and database. Changes get tested on staging before being promoted to production."

**Automated Release Notes**
> "Claude, add automatic release note generation. When a PR is merged to main, automatically generate release notes from the PR title and description. Tag each production deploy with a version number."

**Dependency Updates**
> "Claude, set up Dependabot or Renovate on the GitHub repository to automatically open PRs when dependencies have updates. Configure it to run CI on dependency update PRs so you know if an update breaks anything."

---

## Troubleshooting

**GitHub Actions workflow fails immediately**
> "Claude, the CI workflow failed before running any steps. Check the workflow YAML syntax, the runner configuration (runs-on), and the Node.js setup step."

**Tests fail in CI but pass locally**
> "Claude, tests pass on my machine but fail in GitHub Actions. This is usually an environment difference. Check: (1) Is the test database URL set as a GitHub Secret? (2) Are all required environment variables available in CI? (3) Is the Node.js version the same?"

**Vercel preview deployment isn't created**
> "Claude, the PR doesn't have a Vercel preview deployment. Check the Vercel project settings -- is it connected to the GitHub repository? Is the 'Preview Deployments' setting enabled? Are there any build errors in the Vercel dashboard?"

**Branch protection blocking merges**
> "Claude, I can't merge my PR. Branch protection says checks haven't passed. Check: (1) Is the CI workflow completed? (2) Did all steps pass? (3) Is the status check name in branch protection matching the workflow name? (4) Does the PR have an approval?"

**Lighthouse score won't improve**
> "Claude, the Lighthouse performance score is stuck at [number] despite optimizations. Run Lighthouse with the --view flag to see the full report. What are the remaining issues? Sometimes third-party scripts (Sentry, PostHog) affect the score -- if so, ensure they load asynchronously."

---

## Reflection

TeamTask Pro is complete. Not "tutorial complete" -- production complete. Look at what the application has:

- User authentication with multiple sign-in methods
- Multi-tenant organizations
- Project and task management
- Stripe payment integration
- Email and in-app notifications
- Dashboards with data visualization
- Admin tools
- Activity logging
- Data export
- Automated test suite
- Error monitoring with Sentry
- Product analytics with PostHog
- Feature flags
- Session replay
- Automated CI/CD pipeline
- Preview deployments
- Branch protection
- Performance optimization

You built all of this. Not by writing code, but by understanding what each piece does, describing what you wanted, verifying the output, and shipping it. You researched, planned, directed, and verified your way through a complete SaaS application.

This is the AI-native builder workflow. And TeamTask Pro is proof that it works.

Next week, you start something new -- something you choose. But the skills you built here transfer to anything: understand the tools, describe what you want, verify it works, ship it. That pattern applies to every piece of software, forever.
