# Week 14 Guide: Frat House Frenzy — CI/CD & Production

Until now, your quality pipeline has been manual. You tell Claude Code to run lint, then type-check, then test the payout functions, then run the math simulation, then build. If everything passes, you push and deploy. This works, but it depends entirely on you remembering to do it every time. And humans forget. Especially when you're in a hurry, or when the change seems small, or when it's Friday afternoon and you just want to tweak the cascade animation.

For a game that handles real money, forgetting to run the math simulation before deploying is not "oops." It's a financial risk. CI/CD automates this. Every change gets checked automatically. No human discipline required. The system enforces quality whether you're paying attention or not.

---

## Part 1: CI/CD Concepts

### Continuous Integration (CI)

Continuous Integration means every code change is automatically tested against the quality pipeline. When you push code or open a pull request, an automated system runs your lint, type-check, tests, math simulation, and build steps. If any step fails, you know immediately.

The "continuous" part means this happens on every change, not once in a while. Every push, every PR, every time. This catches problems early, when they're small and easy to fix, instead of late, when they've compounded with other changes.

### Continuous Deployment (CD)

Continuous Deployment means code that passes CI is automatically deployed. For Frat House Frenzy, this means: push to main, CI runs all checks including math verification, and if everything passes, Vercel deploys the new version to production.

You're already doing a version of this -- Vercel deploys automatically when you push to main. But without CI, there's no quality gate. A payout calculation bug gets deployed just as easily as a fix. CI adds the gate.

### The Pipeline for a Game

Here's the complete flow:

```
Push code → Lint → Type-check → Test → Math Verify → Build → Deploy
              ↓         ↓          ↓        ↓            ↓       ↓
           Pass/Fail Pass/Fail  Pass/Fail Pass/Fail   Pass/Fail  Live
```

If any step fails, the pipeline stops. Broken code -- or broken math -- never reaches production. This is the entire point.

---

## Part 2: GitHub Actions for a Game

### What GitHub Actions Are

GitHub Actions is an automation platform built into GitHub. You define workflows -- sequences of steps that run in response to events (like pushing code or opening a PR). The workflows run on GitHub's servers, not on your machine.

A workflow is defined in a YAML file in your repository. You don't need to write YAML by hand -- Claude Code creates and maintains these files. But understanding what they contain helps you give better direction.

### The CI Workflow for Frat House Frenzy

The CI workflow will:
1. Trigger on every push and every pull request
2. Set up Node.js and install dependencies
3. Run lint
4. Run type-check
5. Run unit tests (payout calculations, symbol mapping, Zod validation)
6. Run integration tests (spin flow, provably fair chain)
7. Run a short math simulation (100,000 spins -- fast enough for every PR)
8. Run build

If all steps pass, the PR gets a green check. If any step fails -- including the math simulation -- the PR gets a red X and cannot be merged.

### Why Math Verification Is in CI

In a SaaS app, CI runs lint, tests, and build. That's sufficient. In a game with real money, you also need to verify the math on every change.

Imagine this: a developer changes the cascade logic to fix a visual bug. The change is small, the tests pass, the build succeeds. But the cascade change subtly alters when wins are counted, shifting the RTP from 96.09% to 97.8%. Without math verification in CI, this ships to production and quietly bleeds the bankroll.

The 100,000-spin simulation in CI catches this. It runs in about 10-15 seconds and verifies that RTP, hit frequency, and bonus trigger rate are within expected ranges. It's a fast sanity check, not a full verification -- that comes from the nightly simulation.

### The Nightly Full Simulation

A 100,000-spin simulation catches gross errors but can miss subtle drift. A 10,000,000-spin simulation is much more precise but takes minutes to run -- too slow for every PR.

Solution: run the full simulation nightly via a cron workflow.

```
Schedule: Every night at 2am UTC
Steps:
  1. Check out latest main branch
  2. Run 10,000,000-spin simulation
  3. Verify RTP within ±0.15% of 96.09%
  4. Verify hit frequency within ±1% of 26.3%
  5. Verify bonus trigger rate within ±5% of 1-in-195
  6. Post results to a Slack channel (or create a GitHub issue if thresholds are breached)
```

**Direction for Claude Code:**
> "Create a GitHub Actions cron workflow that runs nightly at 2am UTC. It should run a 10-million-spin simulation and verify that RTP, hit frequency, and bonus trigger rate are within expected ranges. If any metric is outside the threshold, create a GitHub issue with the details."

---

## Part 3: Environment Variables and Secrets

### What Needs to Be Secret

Frat House Frenzy has more sensitive configuration than a typical web app:

- **Database URL** -- Connection string to PostgreSQL where player balances live
- **Stripe keys** -- Live and test keys for processing real deposits and withdrawals
- **Server seed secret** -- The master secret used to generate per-spin server seeds. If this leaks, players can predict outcomes.
- **Sentry DSN** -- Where error reports are sent
- **PostHog API key** -- Where analytics events are sent

### Managing Secrets Across Environments

**Development (.env.local):**
- Local database, test Stripe keys, a development-only seed secret
- This file is in `.gitignore` and never committed

**CI (GitHub Secrets):**
- Test database URL, test Stripe keys, a CI-only seed secret
- Stored in GitHub repository settings, accessed securely by workflows

**Production (Vercel environment variables):**
- Production database, live Stripe keys, the real seed secret
- Set in the Vercel dashboard, encrypted at rest

### The Critical Rules

1. **Never commit secrets.** No API keys, database URLs, seed secrets, or tokens in code files. Ever. A leaked server seed secret means every spin outcome is predictable.
2. **Use separate keys per environment.** Production Stripe keys are different from test keys. The production seed secret is different from the development seed secret.
3. **Rotate compromised keys immediately.** If any key was ever exposed -- even briefly -- generate a new one and revoke the old one.
4. **The seed secret is the crown jewel.** If it leaks, the provably fair system is compromised. Treat it with the same care as a private encryption key.

---

## Part 4: Preview Deployments

### The Concept

A preview deployment gives every pull request its own live URL. Before merging a change into main, you can visit the preview URL and test the changes in a real environment -- with a real game that you can actually spin.

### How It Works with Vercel

Vercel creates preview deployments automatically for every PR:
1. You open a pull request with a change to the cascade animation
2. Vercel builds and deploys the branch to a unique URL
3. The preview URL appears as a comment on the PR
4. You visit the URL and play the game -- spin the reels, trigger cascades, check the animation
5. CI runs in parallel, verifying the math still holds
6. If the preview looks good and CI is green, you merge
7. Vercel deploys to production

### Why Previews Matter for a Game

For a SaaS app, preview deployments let you check that a new feature looks right. For a game, preview deployments let you PLAY the change. You can:
- Feel whether the new animation timing is right
- Test whether the sound effects sync with the cascade
- Verify the bonus round UI works on mobile
- Check that the bet selector responds correctly

Some things you can only evaluate by experiencing them. Preview deployments give you that experience before it goes live.

---

## Part 5: Database Migrations

### Why Migrations Matter More for a Game

Database migrations change the structure of your database: adding tables, modifying columns, creating indexes. Drizzle ORM manages these migrations.

For a SaaS app, a botched migration might mean users temporarily can't create tasks. For Frat House Frenzy, a botched migration could mean:
- **Player balances are lost or corrupted.** If the balance column changes type or gets dropped, players lose real money.
- **Spin history disappears.** Players can no longer verify past spins. The provably fair guarantee is broken.
- **Active sessions are interrupted.** A player mid-bonus-round could lose their progress and their accrued winnings.

### Safe Migration Practices

1. **Always back up before migrating.** Take a database snapshot before running any migration in production.
2. **Make migrations additive.** Add new columns, don't remove old ones. Add new tables, don't rename existing ones. Removing or renaming can break running code during the deployment window.
3. **Test migrations on a copy.** Run the migration against a copy of the production database (with anonymized data) before running it on the real thing.
4. **Never change the balance column type.** If you need a different representation, add a new column and migrate data carefully.
5. **Migration rollbacks.** Every migration should have a rollback path. If something goes wrong, you can undo the change.

### Direction for Claude Code

> "Create a database migration that adds a new column to the spins table for tracking cascade depth. Make sure the migration is additive -- don't modify existing columns. Include a rollback function that removes the new column. Test the migration against the development database before we run it in production."

---

## Part 6: Branch Protection

### Why Branch Protection Matters

Without branch protection, anyone can push directly to the main branch. One bad push and the production game has broken payout logic. Branch protection adds rules that must be satisfied before code can reach production.

### Protection Rules for Frat House Frenzy

**Require CI to pass.** The GitHub Actions workflow -- including the math simulation -- must complete with all green checks before merging. Code that fails the math check never reaches players.

**Require code review.** At least one person must review and approve the pull request. For a game with real money, a second pair of eyes on payout logic changes is essential, not optional.

**Prevent force pushes.** Force pushing to main rewrites history. In a game with an audit trail of changes, this is unacceptable.

**Require branches to be up-to-date.** Before merging, the PR branch must be up-to-date with main. This ensures CI ran against the latest code and the math simulation reflects the current state.

### The Pull Request Workflow

With branch protection, the workflow becomes:
1. Create a new branch for your change
2. Make changes, commit, push the branch
3. Open a pull request
4. CI runs automatically (including math simulation on 100K spins)
5. Test the preview deployment -- play the game
6. Review the code (or self-review for solo projects)
7. Merge when CI is green, the math checks out, and you're satisfied
8. Vercel deploys to production automatically
9. Check the monitoring dashboard to verify the deploy is healthy

### The /ship Skill

You'll create a Claude Code skill that handles the entire shipping workflow with a single command:

> "Claude, create a /ship skill that: creates a feature branch, runs the full test suite including math simulation, reviews the diff, commits, pushes, and opens a pull request. If any check fails, stop and report what went wrong."

The /ship skill encodes the entire safe deployment process. Instead of remembering each step, you type `/ship` and the system handles it.

---

## Part 7: What "Production Ready" Means for a Game

### The Checklist

A SaaS app is production-ready when it works, looks good, and doesn't crash. A game with real money has a higher bar:

**Math verification:**
- [ ] RTP verified within ±0.15% of 96.09% over 10M spins
- [ ] Hit frequency verified within ±1% of 26.3%
- [ ] Bonus trigger rate verified within ±5% of 1-in-195
- [ ] Maximum win cap enforced (42,069x)
- [ ] All symbol payouts match the design brief

**Provably fair:**
- [ ] Server seed commitment happens before spin execution
- [ ] Verification page correctly validates spin outcomes
- [ ] Players can edit their client seed
- [ ] Nonce increments correctly on each spin

**Financial integrity:**
- [ ] Player balances cannot go negative
- [ ] Wins are credited exactly once (no double-credit on retry)
- [ ] Failed spins do not deduct balance
- [ ] Deposits and withdrawals are tracked with full audit trail
- [ ] Stripe webhooks handle all edge cases (failed payments, disputes, refunds)

**Monitoring and alerting:**
- [ ] Sentry configured with source maps and proper alert rules
- [ ] PostHog tracking all key player events
- [ ] Real-time RTP monitoring with deviation alerts
- [ ] Nightly math simulation running via cron

**CI/CD:**
- [ ] GitHub Actions running lint, typecheck, test, math simulation, build on every PR
- [ ] Branch protection requiring CI + review before merge
- [ ] Preview deployments working for every PR
- [ ] Environment variables properly separated (dev/CI/production)
- [ ] No secrets in code or version control

**Performance:**
- [ ] Spin response time under 200ms
- [ ] Game loads in under 3 seconds
- [ ] Animations run at 60fps on target devices
- [ ] Lighthouse Performance score 90+

### The Standard

Production-ready for a game means: the math is verified, the money is safe, the system is monitored, and the deployment pipeline prevents bad code from reaching players. Every item on this checklist is something your CI pipeline, monitoring tools, and testing suite enforce automatically.

You're not checking these boxes by hand. You've built systems that check them for you, every time, on every change. That's what production-ready means.

### Try It: /powerup

These advanced `/powerup` lessons match the production-grade workflows you're building this week:
- **"Worktrees"** -- Run parallel git sessions so you can work on a feature branch while CI runs on another
- **"Code from anywhere"** -- Use `/remote-control` for remote sessions when monitoring production
- **"Schedule recurring tasks"** -- Automate production tasks with `/schedule`

---

## What's Next

Head to [project.md](project.md) to automate the Frat House Frenzy pipeline and make it production-ready.
