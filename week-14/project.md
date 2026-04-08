# Project: Frat House Frenzy — CI/CD & Production

**What you're building:** An automated CI/CD pipeline with GitHub Actions, preview deployments, branch protection, automated math model verification, and a /ship skill. Frat House Frenzy becomes a production-grade game.

**Time estimate:** 8-10 hours

**What you'll need open:** Ghostty (your terminal) and a web browser

---

## The Brief

This is the final week of Frat House Frenzy core development. You're going from "deployed" to "production-ready" -- which means automated quality enforcement, math model verification on every PR, preview deployments for testing changes, branch protection to prevent accidents, and production deployment to Vercel.

When you're done, the entire workflow is automated: push code, CI verifies quality and runs RNG tests, preview deployment lets you test, merge to main, production deploys automatically. No manual steps. No forgotten checks. No broken production. No one accidentally ships a payout bug.

---

## Phase 1: Research

Start Claude Code and research the CI/CD approach:

> "Claude, I have a Next.js slot game (Frat House Frenzy) on GitHub deployed to Vercel. It has a Vitest test suite including unit tests, integration tests, and a long-running math model simulation. I want to set up GitHub Actions CI that runs lint, type-check, test (including the RNG fairness tests), and build on every PR. I also want automated math model verification on PRs, preview deployments, and branch protection on main. What's the best approach?"

Follow up:

> "What does a GitHub Actions workflow file look like for a Next.js project? What are the key decisions -- Node version, caching, parallel vs. sequential jobs?"

> "How do I set up GitHub Secrets for the test database URL, Stripe test keys, and other environment variables the CI pipeline needs?"

> "The full 10M spin simulation takes several minutes. Should I run a shorter simulation in CI (like 100K spins) and save the full one for nightly runs?"

---

## Phase 2: Plan

> "Claude, plan the complete CI/CD and production readiness setup for Frat House Frenzy. I need: (1) GitHub Actions CI workflow that runs lint, typecheck, test (including RNG and payout tests), and build on every push and PR, (2) automated math model verification -- a short simulation (100K spins) on every PR to catch payout regressions, (3) Vercel preview deployments configured for every PR, (4) branch protection on main requiring passing CI and one review, (5) environment variable management for Stripe keys, database URLs, Sentry DSN, and PostHog key, (6) database migration strategy with Drizzle for production, (7) a /ship skill that handles the branch-PR-merge workflow. Plan the order of implementation and any GitHub/Vercel configuration needed."

Review the plan. Key questions:

- What environment variables does CI need, and how are they provided?
- Should CI jobs run in parallel or sequence?
- How long should the CI math simulation take (balancing speed vs. confidence)?
- How does the /ship skill handle the full workflow?
- What's the database migration strategy for production deploys?

---

## Phase 3: Execute

### Step 1: GitHub Actions CI

> "Claude, set up a GitHub Actions CI workflow for Frat House Frenzy. Create a .github/workflows/ci.yml file that: (1) triggers on push to main and on all pull requests, (2) runs on Ubuntu latest with Node.js 20, (3) caches node_modules for faster runs, (4) installs dependencies, (5) runs npm run lint, (6) runs npm run typecheck, (7) runs npm run test (unit and integration tests, including RNG fairness and payout accuracy tests), (8) runs npm run build. Each step should fail the pipeline if it fails. Add a job summary at the end showing which steps passed."

Set up the required secrets:

> "Claude, what GitHub Secrets do I need to add for the CI pipeline? Walk me through adding them in the GitHub repository settings. I need: test database URL, Stripe test secret key, Stripe test webhook secret, Sentry auth token, and any other secrets the tests require."

Go to your GitHub repository > Settings > Secrets and variables > Actions, and add the necessary secrets.

Test the CI pipeline:

> "Claude, create a new branch, make a small change (add a comment to a file), commit, push the branch, and open a pull request. Let's see if CI runs."

Watch the GitHub Actions tab on your repository. Verify:
- The workflow triggers automatically
- All steps run in order
- All steps pass with green checks
- The PR shows the CI status

### Step 2: Automated Math Model Verification

> "Claude, add a math model verification step to the CI pipeline. Create a separate job in the GitHub Actions workflow called 'math-verification' that: (1) runs after the main CI job passes, (2) executes a short math simulation -- 100,000 spins, (3) verifies the observed RTP is within 2% of the target 96.09% (wider tolerance for the smaller sample), (4) verifies the hit frequency is within 5% of the expected 26.3%, (5) outputs the results in the job summary, (6) fails the PR if the RTP is outside the acceptable range. This catches payout bugs before they reach production."

> "Claude, also create a separate workflow file .github/workflows/nightly-math.yml that runs the full 10M spin simulation on a nightly schedule (cron). This gives us high-confidence RTP verification without slowing down every PR."

Test it:

> "Claude, push a change to the open PR and verify the math verification job runs and passes."

### Step 3: Environment Variable Management

> "Claude, set up environment variable management for Frat House Frenzy across all environments. Create: (1) a .env.example file listing every required variable with placeholder values and comments explaining each one, (2) a .env.test file for the test environment (test database URL, Stripe test keys), (3) documentation in the README for which variables need to be set in Vercel for production. The required variables are: DATABASE_URL, STRIPE_SECRET_KEY, STRIPE_WEBHOOK_SECRET, NEXT_PUBLIC_STRIPE_PUBLISHABLE_KEY, SENTRY_DSN, SENTRY_AUTH_TOKEN, NEXT_PUBLIC_POSTHOG_KEY, POSTHOG_API_SECRET, BETTER_AUTH_SECRET, NEXT_PUBLIC_APP_URL. Make sure .env files with real values are in .gitignore."

### Step 4: Database Migration Strategy

> "Claude, set up a production database migration strategy using Drizzle ORM. Configure: (1) a drizzle.config.ts that reads the DATABASE_URL from environment variables, (2) npm scripts for 'db:generate' (generate migration files), 'db:migrate' (apply migrations), and 'db:push' (push schema changes directly for development), (3) add the migration step to the CI pipeline -- after tests pass and before build, run migrations against the production database on merges to main, (4) add migration files to version control so every schema change is tracked. Walk me through how to handle a schema change safely: generate the migration, review it, test it, then deploy."

Test it:

> "Claude, make a small schema change (add an optional column to a table), generate the migration, review it, and verify it applies correctly in the test environment."

### Step 5: Preview Deployments

Vercel preview deployments are likely already working if your repo is connected to Vercel. Verify:

> "Claude, is the PR I just created showing a Vercel preview deployment? Check the PR comments or the Vercel dashboard."

If preview deployments aren't working:

> "Claude, configure Vercel to create preview deployments for every PR on the Frat House Frenzy repository. Walk me through the Vercel dashboard settings. Make sure preview deployments have the correct environment variables (test Stripe keys, preview database URL)."

Visit the preview deployment URL from the PR. Verify:
- The preview loads correctly
- It uses the PR's code changes
- The game is playable (spin, bet, view history)
- Stripe test mode works for deposits

### Step 6: Branch Protection

> "Claude, walk me through setting up branch protection on the main branch. I want to require: (1) the CI workflow to pass before merging (including the math verification job), (2) at least one approval on the PR (I'll self-approve for now), (3) the branch to be up-to-date with main before merging, (4) no force pushes to main."

Go to your GitHub repository > Settings > Branches > Add branch protection rule:
- Branch name pattern: `main`
- Require status checks to pass before merging: enable, select the CI workflow and math-verification job
- Require pull request reviews before merging: enable, require 1 approval
- Require branches to be up-to-date before merging: enable
- Block force pushes: enable

Test it:

> "Claude, try to push directly to main. It should be blocked."

> "Claude, merge the test PR (approve it first if needed). Verify CI and math verification both pass and the merge succeeds."

### Step 7: Production Deployment to Vercel

> "Claude, verify the production deployment pipeline. After merging the PR to main: (1) check the Vercel dashboard -- did a production deployment start automatically? (2) verify the deployment completes successfully, (3) visit the production URL and play a few spins to verify the game works, (4) check Sentry -- does a new release appear? (5) check PostHog -- are production events flowing?"

> "Claude, walk me through the Vercel project settings. Verify: the production branch is 'main', the build command is correct, the output directory is correct, and all production environment variables are set (DATABASE_URL pointing to production database, live Stripe keys, Sentry DSN, PostHog key)."

### Step 8: The /ship Skill

> "Claude, create a /ship skill in CLAUDE.md that handles the full shipping workflow: (1) Create a new branch with a descriptive name based on the changes, (2) Commit all staged changes, (3) Push the branch, (4) Open a pull request with a clear title and description summarizing the changes, (5) Wait for CI and math verification to pass, (6) Show the preview deployment URL, (7) After review, merge the PR. The skill should handle each step and report status along the way."

Test the skill:

> "Claude, make a small improvement to the game (fix a typo, adjust spacing, improve an error message). Then use /ship to ship it."

Verify:
- The branch gets created
- The PR opens with a good description
- CI runs and passes (including RNG tests)
- Math verification runs and passes
- The preview deployment is accessible and the game works
- The merge succeeds
- Production deploys automatically

### Step 9: Final Quality Check

> "Claude, run the full quality pipeline one final time: lint, typecheck, test, build. Everything must pass."

> "Claude, commit any remaining changes, push, and deploy."

Verify on the production URL:
- The game loads and is playable
- Deposits work (Stripe test mode in staging, live in production)
- Spins produce valid results
- Provably fair verification works
- Sentry is monitoring
- PostHog events are flowing
- The CI pipeline is green

---

## Acceptance Criteria

Your project is complete when all of these are true:

- [ ] GitHub Actions CI workflow runs lint, typecheck, test (including RNG tests), and build on every push and PR
- [ ] CI passes on the main branch
- [ ] Automated math model verification (100K spin simulation) runs on every PR
- [ ] Nightly math verification (10M spin simulation) is configured
- [ ] Vercel preview deployments are created for every PR
- [ ] Branch protection is enabled on main (require CI + math verification + review)
- [ ] Direct pushes to main are blocked
- [ ] Environment variables are documented in .env.example
- [ ] GitHub Secrets are configured for CI (no secrets in code)
- [ ] Database migration strategy is set up with Drizzle (generate, migrate, version control)
- [ ] Production deployment to Vercel works automatically on merge to main
- [ ] Production environment variables are configured in Vercel
- [ ] A /ship skill handles the full branch-PR-merge workflow
- [ ] All quality gates pass (lint, typecheck, test, build)
- [ ] Code is on GitHub
- [ ] App is deployed and functional on Vercel

---

## Stretch Goals

**Lighthouse CI in GitHub Actions**
> "Claude, add a Lighthouse CI step to the GitHub Actions workflow that runs a Lighthouse audit on the preview deployment URL. Fail the CI pipeline if any Lighthouse score drops below 85. This prevents performance regressions from being merged -- especially important for the game's animation performance."

**Staging Environment**
> "Claude, set up a staging environment for Frat House Frenzy. Create a 'staging' branch that auto-deploys to a separate Vercel deployment with its own environment variables, database, and Stripe test keys. Changes get tested on staging before being promoted to production."

**Automated Release Notes**
> "Claude, add automatic release note generation. When a PR is merged to main, automatically generate release notes from the PR title and description. Tag each production deploy with a version number. Include the math verification results in the release notes."

**Dependency Updates**
> "Claude, set up Dependabot or Renovate on the GitHub repository to automatically open PRs when dependencies have updates. Configure it to run CI (including RNG tests) on dependency update PRs so you know if a library update breaks the game engine."

---

## Troubleshooting

**GitHub Actions workflow fails immediately**
> "Claude, the CI workflow failed before running any steps. Check the workflow YAML syntax, the runner configuration (runs-on), and the Node.js setup step."

**Tests fail in CI but pass locally**
> "Claude, tests pass on my machine but fail in GitHub Actions. This is usually an environment difference. Check: (1) Is the test database URL set as a GitHub Secret? (2) Are all required environment variables available in CI? (3) Is the Node.js version the same? (4) Are Stripe test keys configured?"

**Math verification fails on PR**
> "Claude, the math model verification job failed -- it says the RTP is outside the acceptable range. Check: (1) Did the PR change any payout logic or symbol weights? (2) Is the 100K spin sample size producing too much variance? (3) Run the full 10M simulation locally to get a more accurate reading. If the RTP genuinely shifted, the payout logic needs fixing before merging."

**Vercel preview deployment isn't created**
> "Claude, the PR doesn't have a Vercel preview deployment. Check the Vercel project settings -- is it connected to the GitHub repository? Is the 'Preview Deployments' setting enabled? Are there any build errors in the Vercel dashboard?"

**Database migration fails in production**
> "Claude, the migration failed during deployment. Check: (1) Is the DATABASE_URL in Vercel pointing to the production database? (2) Does the migration have destructive changes (dropping columns/tables)? (3) Is there a data conflict? Never run destructive migrations without a backup. Walk me through rolling back safely."

**Branch protection blocking merges**
> "Claude, I can't merge my PR. Branch protection says checks haven't passed. Check: (1) Is the CI workflow completed? (2) Did all steps pass, including math verification? (3) Is the status check name in branch protection matching the workflow/job name? (4) Does the PR have an approval?"

---

## Reflection

Frat House Frenzy is complete. Not "tutorial complete" -- production complete. Look at what the game has:

- Player authentication with Better Auth
- Deposits and withdrawals with Stripe
- A provably fair game engine with HMAC-SHA256 verification
- 5x4 cascading reels with expanding grids up to 5x7
- Multiple bonus modes with escalation mechanics
- xNudge Wilds, Mystery Symbols, and the Tip Jar Collector
- A math model verified across 10 million simulations
- Real-time RTP monitoring across all players
- Error monitoring with Sentry
- Player analytics with PostHog
- Automated test suite with statistical verification
- CI/CD pipeline with math model verification on every PR
- Preview deployments for safe testing
- Branch protection to prevent accidents
- Production deployment to Vercel

You built all of this. Not by writing code, but by understanding what each piece does, describing what you wanted, verifying the output, and shipping it. You researched, planned, directed, and verified your way through a complete production game.

This is the AI-native builder workflow. And Frat House Frenzy is proof that it works.

Next week, you start extending the game with advanced features. But the skills you built here transfer to anything: understand the tools, describe what you want, verify it works, ship it. That pattern applies to every piece of software, forever.
