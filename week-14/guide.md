# Week 14 Guide: CI/CD and Production Readiness

Until now, your quality pipeline has been manual. You tell Claude Code to run lint, then type-check, then test, then build. If everything passes, you push and deploy. This works, but it depends entirely on you remembering to do it every time. And humans forget. Especially when you're in a hurry, or when the change seems small, or when it's Friday afternoon.

CI/CD automates this. Every change gets checked automatically. No human discipline required. The system enforces quality whether you're paying attention or not.

---

## Part 1: CI/CD Concepts

### Continuous Integration (CI)

Continuous Integration means every code change is automatically tested against the quality pipeline. When you push code or open a pull request, an automated system runs your lint, type-check, test, and build steps. If any step fails, you know immediately.

The "continuous" part means this happens on every change, not once in a while. Every push, every PR, every time. This catches problems early, when they're small and easy to fix, instead of late, when they've compounded with other changes.

### Continuous Deployment (CD)

Continuous Deployment means code that passes CI is automatically deployed. For TeamTask Pro, this means: push to main, CI runs all checks, and if everything passes, Vercel deploys the new version to production.

You're already doing a version of this -- Vercel deploys automatically when you push to main. But without CI, there's no quality gate. Broken code gets deployed just as easily as working code. CI adds the gate.

### The Pipeline

Here's the complete flow:

```
Push code → Lint → Type-check → Test → Build → Deploy
              ↓         ↓          ↓       ↓        ↓
           Pass/Fail Pass/Fail  Pass/Fail Pass/Fail  Live
```

If any step fails, the pipeline stops. Broken code never reaches production. This is the entire point.

---

## Part 2: GitHub Actions

### What GitHub Actions Are

GitHub Actions is an automation platform built into GitHub. You define workflows -- sequences of steps that run in response to events (like pushing code or opening a PR). The workflows run on GitHub's servers, not on your machine.

A workflow is defined in a YAML file in your repository. You don't need to write YAML by hand -- Claude Code creates and maintains these files. But understanding what they contain helps you give better direction.

### How Workflows Work

A workflow has:
- **Trigger:** What event starts it (push to main, open a PR, manual trigger)
- **Jobs:** One or more groups of steps that run in parallel or sequence
- **Steps:** Individual commands (install dependencies, run lint, run tests)
- **Environment:** What machine the workflow runs on (usually Ubuntu Linux)

For TeamTask Pro, the CI workflow will:
1. Trigger on every push and every pull request
2. Set up Node.js and install dependencies
3. Run lint
4. Run type-check
5. Run tests
6. Run build

If all steps pass, the PR gets a green check. If any step fails, the PR gets a red X.

### Secrets in GitHub Actions

Your CI pipeline needs access to environment variables (database URL, Sentry DSN, etc.) but those values can't be in the code. GitHub Actions has a Secrets feature: you store sensitive values in the repository settings, and the workflow accesses them securely at runtime.

---

## Part 3: Preview Deployments

### The Concept

A preview deployment gives every pull request its own live URL. Before merging a change into main, you (or anyone on the team) can visit the preview URL and test the changes in a real environment.

This is powerful because it catches issues that only appear in a deployed environment. Something might work perfectly on your local machine but break when deployed -- different environment variables, different server configuration, different behavior. Preview deployments catch these problems before they hit production.

### How It Works with Vercel

Vercel creates preview deployments automatically for every PR. When you open a pull request:
1. Vercel detects the new branch
2. Vercel builds and deploys the branch to a unique URL (something like `teamtask-pro-git-feature-xyz.vercel.app`)
3. The preview URL appears as a comment on the PR
4. You test the changes at that URL
5. If everything looks good, you merge the PR
6. Vercel deploys the merged code to production

No configuration needed beyond connecting your GitHub repo to Vercel, which you've already done.

---

## Part 4: Branch Protection

### Why Branch Protection Matters

Without branch protection, anyone can push directly to the main branch. One bad push and production is broken. Branch protection adds rules that must be satisfied before code can be merged into main.

### Common Protection Rules

**Require CI to pass.** The GitHub Actions workflow must complete with all green checks before merging is allowed. This prevents merging code that fails lint, type-check, tests, or build.

**Require a code review.** At least one person must review and approve the pull request before merging. For a solo project, this might seem unnecessary, but it establishes the habit. And even solo, a PR gives you a moment to review your own changes as a batch before they go to production.

**Prevent force pushes.** Force pushing rewrites history and can destroy other people's work. Block it on main.

**Require branches to be up-to-date.** Before merging, the PR branch must be up-to-date with main. This prevents merge conflicts and ensures CI ran against the latest code.

### The Pull Request Workflow

With branch protection, the workflow becomes:
1. Create a new branch for your change
2. Make changes, commit, push the branch
3. Open a pull request
4. CI runs automatically
5. Review the preview deployment
6. Get approval (or self-review for solo projects)
7. Merge when CI is green and you're satisfied
8. Vercel deploys to production automatically

This is how every professional software team operates. The habits you build here transfer directly to any team you join.

---

## Part 5: Environment Management

### Why Separate Environments

You don't want to test on the same system your real users are using. Separate environments isolate different stages of development:

**Development** (your local machine) -- Where you build and experiment. Uses a local database, test API keys, and localhost.

**Staging** (optional) -- A deployment that mirrors production but isn't public. Used for final testing before releasing to real users.

**Production** -- The live application that real users interact with. Uses real databases, real API keys, and real money (for payments).

### Environment Variables Per Environment

Each environment has its own set of variables:
- Development: local database, test Stripe keys, test Sentry DSN
- Production: production database, live Stripe keys, production Sentry DSN

Vercel handles this through its environment variable settings, where you can set different values for "Production," "Preview," and "Development."

### The Critical Rule

Never use production credentials in development. Never test with real payment methods. Never point development code at the production database. Environments exist to prevent accidents.

---

## Part 6: Performance Basics

### Why Performance Matters

Slow websites lose users. Studies consistently show that even small delays (a few hundred milliseconds) increase bounce rates. Google uses page speed as a ranking factor. Performance isn't a nice-to-have; it directly affects whether people use your product.

### Lighthouse

Lighthouse is Google's tool for auditing web performance. It runs in Chrome DevTools or as a command-line tool and measures:

- **Performance** -- How fast does the page load and become interactive?
- **Accessibility** -- Can people with disabilities use the page?
- **Best Practices** -- Does the page follow web development best practices?
- **SEO** -- Is the page optimized for search engines?

Each category gets a score from 0 to 100. You're aiming for green (90+) on all categories.

### Core Web Vitals

Core Web Vitals are three specific metrics Google uses to measure user experience:

**Largest Contentful Paint (LCP)** -- How long until the largest visible element loads. Measures loading speed. Target: under 2.5 seconds.

**Interaction to Next Paint (INP)** -- How long the page takes to respond when a user clicks or types. Measures interactivity. Target: under 200 milliseconds.

**Cumulative Layout Shift (CLS)** -- How much the page layout shifts while loading. Measures visual stability. Target: under 0.1. (This is that annoying experience when a page loads and everything jumps around as ads and images pop in.)

### Image Optimization

Images are almost always the biggest performance problem. An unoptimized hero image can be 5MB. Optimized, it might be 100KB -- the same visual quality at 2% of the file size.

Next.js has a built-in Image component that handles optimization automatically: correct sizing, modern formats (WebP, AVIF), lazy loading, and responsive sizes. Using it instead of raw `<img>` tags is one of the highest-impact performance improvements you can make.

### Caching

Caching means storing the result of an expensive operation so you don't redo it every time. Two types matter for web apps:

**Browser caching** -- The browser stores static assets (images, CSS, JavaScript) locally. On subsequent visits, the browser uses the cached version instead of downloading again. Proper cache headers make this happen automatically.

**Database query caching** -- Some database queries are expensive (aggregating dashboard data across thousands of tasks). Caching the result for a short time (even 30 seconds) means the query runs once and serves many requests.

### Lazy Loading

Not everything on the page needs to load immediately. Lazy loading means deferring the loading of off-screen content until the user scrolls to it. Images below the fold, charts that aren't visible yet, heavy components that aren't in the initial viewport -- all candidates for lazy loading.

---

## Part 7: Secrets Management

### The Problem

Your application needs sensitive values: database URLs, API keys, webhook secrets. These values must be available to the application at runtime but must never appear in your code repository.

If an API key gets committed to a public GitHub repo, bots find it within minutes and exploit it. This is not theoretical -- it happens constantly.

### The Solution

**Environment variables** store secrets outside the code. Your `.env.local` file (for development) is in `.gitignore` and never gets committed. Vercel's environment variable settings (for production) are encrypted and access-controlled.

**GitHub Secrets** store values needed by CI. The test suite might need a test database URL. You add it as a GitHub Secret, and the CI workflow accesses it securely.

### Best Practices

1. **Never commit secrets.** No API keys, database URLs, or tokens in code files. Ever.
2. **Use .env.local for development.** Add it to `.gitignore`.
3. **Use Vercel environment variables for production.** Set them in the Vercel dashboard.
4. **Use GitHub Secrets for CI.** Set them in the GitHub repository settings.
5. **Rotate compromised keys immediately.** If a key was ever exposed, generate a new one and revoke the old one.
6. **Use separate keys per environment.** Production Stripe keys are different from test Stripe keys.

### Try It: /powerup

These advanced `/powerup` lessons match the production-grade workflows you're building this week:
- **"Worktrees"** -- Run parallel git sessions so you can work on a feature branch while CI runs on another
- **"Code from anywhere"** -- Use `/remote-control` for remote sessions when monitoring production
- **"Schedule recurring tasks"** -- Automate production tasks with `/schedule`

---

## What's Next

Head to [project.md](project.md) to automate the TeamTask Pro pipeline and make it production-ready.
