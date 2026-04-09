# Accelerated Track: 10 Weeks

> You're already building. This gets you to production faster.

---

## What This Is

The standard curriculum is 20 weeks, linear, and designed for someone who's never built anything. That's not you. You're already using Claude Code, you have a project (Frat House Frenzy), and you're learning by doing.

This is the same material compressed into 10 weeks. You skip the slow ramp. You pull from curriculum guides *when you need them*, not in order. You're building FHF the entire time — the project *is* the curriculum.

The rules are the same: you direct Claude Code, you verify with quality gates, you ship to production. You just move faster.

---

## The 10-Week Map

### Week 1-2: FHF Core

**What you ship:** Auth, database, basic UI, project scaffolding, deployed to Vercel.

**Pull from the curriculum:**
- [week-01/guide.md](./week-01/guide.md) — Environment setup, Git workflow, Claude Code basics
- [week-02/guide.md](./week-02/guide.md) — Claude Code workflow patterns
- [week-03/guide.md](./week-03/guide.md) — CLAUDE.md, skills, hooks
- [week-04/guide.md](./week-04/guide.md) — Next.js, TypeScript, Tailwind, shadcn/ui
- [week-05/guide.md](./week-05/guide.md) — PostgreSQL, Drizzle, Zod, API routes
- [week-06/guide.md](./week-06/guide.md) — Better Auth, sessions
- [week-08/project.md](./week-08/project.md) — FHF Foundation project brief

**Skills absorbed:** Next.js, TypeScript, Tailwind CSS, shadcn/ui, Better Auth, PostgreSQL, Drizzle ORM, Git, Vercel, Claude Code workflow.

**End state:** FHF repo exists, CLAUDE.md written, auth works, database schema deployed, player can sign up and log in, basic layout renders, live on Vercel.

---

### Week 3: Game Engine

**What you ship:** Provably fair RNG, symbol system, ways-to-win evaluation, cascading wins.

**Pull from the curriculum:**
- [week-10/guide.md](./week-10/guide.md) — Provably fair concepts, RNG architecture
- [week-10/project.md](./week-10/project.md) — Game engine project brief

**Skills absorbed:** Node.js crypto (HMAC-SHA256), API routes, Zod validation, server-side game logic, provably fair verification.

**End state:** Server generates cryptographically fair outcomes. Client can verify every spin. Cascading wins resolve correctly. Symbol mapping and paytable exist. All game logic is server-side with no client trust.

---

### Week 4: Reel UI + Animations + Sound

**What you ship:** Spinning reels, win animations, cascading visual effects, game audio.

**Pull from the curriculum:**
- [week-11/guide.md](./week-11/guide.md) — Reel UI architecture, animation patterns
- [week-11/project.md](./week-11/project.md) — Reel UI project brief

**Skills absorbed:** React component architecture, Framer Motion, Howler.js, complex state management, animation sequencing.

**End state:** Reels spin and land. Wins highlight with animations. Cascading wins visually resolve. Sound effects and music play. The game *feels* like a game.

---

### Week 5: Payments + Wallet

**What you ship:** Stripe deposits, bet system, balance management, withdrawal flow.

**Pull from the curriculum:**
- [week-09/guide.md](./week-09/guide.md) — Stripe integration, webhook handling
- [week-09/project.md](./week-09/project.md) — Wallet & Payments project brief

**Skills absorbed:** Stripe integration, webhooks, financial transaction handling, idempotency, ledger patterns.

**End state:** Players can deposit real money via Stripe. Bets deduct from balance. Wins credit to balance. Transaction history is accurate. Webhook handling is bulletproof.

---

### Week 6: v1 Launch + Admin Dashboard

**What you ship:** Admin dashboard, production polish, public launch.

**Pull from the curriculum:**
- [week-11/project.md](./week-11/project.md) — Admin dashboard and v1 launch section
- [week-07/guide.md](./week-07/guide.md) — Deployment polish, production readiness
- [week-07/project.md](./week-07/project.md) — Production deployment patterns

**Skills absorbed:** Data visualization (Recharts), admin patterns, production deployment, quality gates.

> **MILESTONE:** Playable game. Live URL. Real auth. Real payments. Provably fair. Someone can find it, sign up, deposit, play, and withdraw. This is v1.

---

### Week 7: Testing + Monitoring + CI/CD

**What you ship:** Math model tests, RNG fairness verification, Sentry, PostHog, GitHub Actions pipeline.

**Pull from the curriculum:**
- [week-12/guide.md](./week-12/guide.md) — Vitest, math model testing
- [week-12/project.md](./week-12/project.md) — Testing project brief
- [week-13/guide.md](./week-13/guide.md) — Sentry, PostHog setup
- [week-13/project.md](./week-13/project.md) — Monitoring project brief
- [week-14/guide.md](./week-14/guide.md) — GitHub Actions, CI/CD
- [week-14/project.md](./week-14/project.md) — CI/CD project brief

**Skills absorbed:** Vitest, math model verification, RTP simulation, Sentry error tracking, PostHog analytics, GitHub Actions, automated quality gates.

This is three standard weeks compressed into one. It works because testing, monitoring, and CI/CD reinforce each other — you write the tests, wire them into CI, and add monitoring in a single pass.

**End state:** Math model verified (10k+ simulated spins, RTP within tolerance). Sentry catches errors. PostHog tracks player behavior. Every push runs lint, typecheck, test, build automatically. You sleep at night.

---

### Week 8: Advanced Mechanics

**What you ship:** xNudge wilds, Beer Pong Respin, Party Foul events, bonus rounds, Blackout Meter.

**Pull from the curriculum:**
- [week-15/guide.md](./week-15/guide.md) — Advanced game mechanics
- [week-15/project.md](./week-15/project.md) — Advanced mechanics project brief
- [week-16/guide.md](./week-16/guide.md) — Bonus rounds, escalation
- [week-16/project.md](./week-16/project.md) — Bonus rounds project brief

**Skills absorbed:** Complex state management, animation sequencing, bonus round logic, sound design, escalation mechanics.

**End state:** The game has depth. Multiple special features trigger and resolve. Bonus rounds work. The experience goes from "functional slot game" to "game people want to play again."

---

### Week 9: Speed Build — Second Game

**What you ship:** A second game — crash, mines, or a slots variant — idea to production in 5 days.

**Pull from the curriculum:**
- [week-17/guide.md](./week-17/guide.md) — Package extraction, code reuse patterns
- [week-18/guide.md](./week-18/guide.md) — Speed build methodology
- [week-18/project.md](./week-18/project.md) — Speed build project brief

**Skills absorbed:** Code reuse, rapid prototyping, npm package extraction, knowing which corners to cut and which to never cut.

**End state:** Second game is live. You proved you can ship fast by reusing the engine, auth, payments, and monitoring infrastructure you already built.

---

### Week 10: Portfolio + Launch

**What you ship:** Portfolio site, retrospective, professional packaging — everything presentable.

**Pull from the curriculum:**
- [week-07/project.md](./week-07/project.md) — Portfolio site patterns
- [week-19/guide.md](./week-19/guide.md) — Portfolio assembly
- [week-19/project.md](./week-19/project.md) — Portfolio assembly project brief
- [week-20/guide.md](./week-20/guide.md) — Retrospective, professional identity
- [week-20/project.md](./week-20/project.md) — Final project brief

**Skills absorbed:** Portfolio presentation, retrospective writing, professional packaging, narrative framing.

**End state:** Portfolio site is live with 3+ deployed projects. Retrospective is written and published. Everything is linked, polished, and ready to show anyone.

---

## Just-In-Time Reference

You don't read these in order. You read them when you hit the concept while building.

| When you're working on... | Look up... |
|---------------------------|------------|
| Setting up a new project | [week-01/guide.md](./week-01/guide.md) — environment, Git init, first deploy |
| Writing CLAUDE.md for your repo | [week-03/guide.md](./week-03/guide.md) — CLAUDE.md patterns, skills, hooks |
| Next.js routing, layouts, components | [week-04/guide.md](./week-04/guide.md) — modern web stack |
| Database schema, migrations | [week-05/guide.md](./week-05/guide.md) — PostgreSQL, Drizzle, Zod |
| API routes, server actions | [week-05/guide.md](./week-05/guide.md) — data layer patterns |
| Auth, sessions, protected routes | [week-06/guide.md](./week-06/guide.md) — Better Auth |
| Stripe, payments, webhooks | [week-09/guide.md](./week-09/guide.md) — wallet & payments |
| RNG, provably fair, crypto | [week-10/guide.md](./week-10/guide.md) — game engine |
| Animations, Framer Motion | [week-11/guide.md](./week-11/guide.md) — reel UI |
| Sound, Howler.js | [week-11/guide.md](./week-11/guide.md) — sound design section |
| Testing, Vitest | [week-12/guide.md](./week-12/guide.md) — testing & quality |
| Sentry, error tracking | [week-13/guide.md](./week-13/guide.md) — monitoring |
| PostHog, analytics | [week-13/guide.md](./week-13/guide.md) — analytics |
| GitHub Actions, CI/CD | [week-14/guide.md](./week-14/guide.md) — CI/CD pipeline |
| Advanced game mechanics | [week-15/guide.md](./week-15/guide.md), [week-16/guide.md](./week-16/guide.md) |
| Code reuse, package extraction | [week-17/guide.md](./week-17/guide.md) — advanced patterns |
| Claude Code patterns | [reference/claude-code-patterns.md](./reference/claude-code-patterns.md) |
| Stack quick reference | [reference/stack-guide.md](./reference/stack-guide.md) |
| Quality gate commands | [reference/quality-gates.md](./reference/quality-gates.md) |
| Game design reference | [reference/game-design-brief.md](./reference/game-design-brief.md) |

---

## Non-Negotiable Checkpoints

You can move fast. You can skip ahead. You can pull from any week in any order. But these are the things you never skip, regardless of pace.

### Quality gates on every significant change
Run `lint`, `typecheck`, `test`, `build` before every commit that matters. Not just before deploy — before every PR merge. This is the habit that separates shipping from hoping.

### CLAUDE.md for the project repo
Your FHF repo gets a CLAUDE.md on day one. It tells Claude Code your stack, your conventions, your database schema shape, your project structure. Update it as the project evolves. This is how you give Claude Code context without repeating yourself every session.

### Git discipline
Branches for features. Commits that describe what changed and why. Pull requests, not pushes to main. Code review (even if you're reviewing your own PRs). This is non-negotiable even when you're the only developer — especially when you're the only developer.

### Testing the math model
Before v1 launch: simulate 10,000+ spins and verify RTP is within expected tolerance. Test edge cases — max bet, zero balance, concurrent spins. The math must be provably correct, not "seems right." This is a real-money game.

### Provably fair implementation
Server commits to outcomes before the player sees them. Player can verify any spin after the fact. HMAC-SHA256, server seed, client seed, nonce. This isn't optional — it's the entire trust model.

### Sentry + PostHog monitoring before v1 launch
You don't launch without knowing when things break (Sentry) and how people use the product (PostHog). Set these up before going live, not after the first bug report from a user.

### CI/CD pipeline before calling it production
If deploying requires you to remember steps, it's not production. GitHub Actions runs your quality gates on every push. Merge to main triggers deploy. Automated, reproducible, no manual steps.

---

## Skills Learned By Building

Everything the standard curriculum teaches across 20 weeks, you absorb by building FHF. Here's the mapping.

| Standard Curriculum Skill | How You Learn It Building FHF |
|--------------------------|-------------------------------|
| Terminal / CLI basics | Every Claude Code session |
| Git workflow | Branching, committing, PRs for every feature |
| Claude Code direction | Every task — the whole project is AI-directed |
| CLAUDE.md authoring | Writing and maintaining FHF's CLAUDE.md |
| Next.js (routing, layouts, SSR) | FHF app structure, pages, API routes |
| TypeScript | Every file in the project |
| Tailwind CSS | All UI styling |
| shadcn/ui components | Buttons, dialogs, forms, data tables |
| PostgreSQL + Drizzle | Player accounts, wallet transactions, game history |
| Zod validation | API input validation, game state verification |
| Better Auth | Player registration, login, sessions |
| Stripe integration | Deposits, withdrawals, webhook handling |
| API design | Game engine endpoints, wallet endpoints |
| React component architecture | Reel components, game board, admin panels |
| State management | Game state, animation state, wallet state |
| Framer Motion | Reel spins, win animations, cascading effects |
| Howler.js / sound design | Spin sounds, win sounds, ambient music |
| Node.js crypto | HMAC-SHA256, server seeds, provably fair verification |
| Vitest / testing | Math model verification, RNG fairness, API tests |
| Sentry / error monitoring | Production error tracking for live game |
| PostHog / analytics | Player behavior, funnel analysis, RTP monitoring |
| GitHub Actions / CI/CD | Automated lint, typecheck, test, build, deploy |
| Data visualization | Admin dashboard with Recharts |
| Production deployment | Vercel config, environment variables, domains |
| Code reuse / package extraction | Game engine as reusable module for second game |
| Speed building | Second game in one week |
| Portfolio presentation | Packaging FHF + second game + portfolio site |

The standard curriculum introduces these one at a time across 20 weeks. You hit all of them in 10 weeks because you're building something real that *requires* all of them.

---

## How to Use This

1. Start at Week 1-2. Set up FHF.
2. When you hit a concept you don't understand, check the Just-In-Time Reference table.
3. Read the relevant guide — not the whole thing, just the section you need.
4. Direct Claude Code to build it. Verify with quality gates. Ship.
5. Move to the next week when the "end state" is met.
6. Never skip a Non-Negotiable Checkpoint.

You're not following a tutorial. You're building a product and using the curriculum as a reference manual.

Let's build.
