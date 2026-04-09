# Week 13 Guide: Frat House Frenzy — Monitoring & Analytics

Your game is deployed. Players can sign up, deposit real money, and spin the reels. But you're flying blind. If the payout calculation breaks at 2am, you won't know until your bankroll is drained or players start posting angry reviews. If nobody triggers the bonus round because the scatter frequency is wrong, you have no way to tell. If the game crashes on Android Chrome, you might not discover that for weeks.

This week you add two kinds of visibility: monitoring (is the game working?) and analytics (how are players using it?). For a game with real money, both are non-negotiable.

---

## Part 1: Why Monitoring Matters More for a Game

### The Cost of Invisible Failures

In a SaaS app, an undetected error means a user can't complete a task. Annoying, but nobody loses money. In Frat House Frenzy, an undetected error can mean:

1. **Payout calculation bug** -- Players receive 10x what they should. Your bankroll hemorrhages.
2. **Balance mutation error** -- A win is credited twice. Or not credited at all.
3. **RNG failure** -- The random number generator falls back to a default, producing predictable or biased outcomes.
4. **Provably fair break** -- The seed commitment doesn't match the revealed seed. Players can't verify their spins. Trust is destroyed.

Every one of these is a financial or legal problem. In a SaaS app, you fix bugs when users report them. In a game with real money, you need to know about bugs before players do.

### What "Monitoring" Means for a Game

Monitoring answers: **Is my game engine working correctly right now?**

It's not about page views or user counts. It's about errors, crashes, and failures in the systems that handle money. When something goes wrong in production, monitoring tells you immediately -- what broke, where it broke, which players were affected, and what they were doing when it happened.

---

## Part 2: Sentry for Error Tracking

### Why Sentry

Sentry catches errors wherever they happen -- in the browser during animation rendering, in API routes during spin processing, in background jobs during balance reconciliation. Each error report includes context that makes it actionable.

### What to Track with Sentry in Frat House Frenzy

**Game engine errors.** Any exception in the spin pipeline: RNG generation, symbol mapping, payout calculation, balance mutation. These are the most critical errors in the system. A TypeError in the payout function isn't just a bug -- it's a financial incident.

Each error report includes:
- **The error message and stack trace** -- What went wrong and where in the code
- **Player context** -- Which player was affected, their current balance, the bet amount
- **Spin context** -- The server seed, client seed, nonce, and resulting symbols (for debugging the exact spin that failed)
- **Breadcrumbs** -- A timeline of events leading up to the error (deposit, bet placed, spin started, cascade triggered, error)
- **Request data** -- The full API request that triggered the error

**Payment failures.** Any error in the Stripe integration: failed deposits, failed withdrawals, webhook processing errors. Money going in and out of the system must be tracked meticulously.

**API errors.** Any 500-level response from your API routes. These indicate server-side failures that players experience as "something went wrong."

### Source Maps

When your Next.js application is deployed, the code is compiled and minified. A production error says "error at line 1, column 47823 of chunk-abc123.js." That tells you nothing.

Source maps map compiled code back to your original source files. With source maps configured, Sentry shows "error at line 42 of src/lib/game-engine/payout.ts" -- the exact file and line in your actual code. Without source maps, production errors are nearly useless.

### Alerts That Matter

Not all errors are equal. Configure Sentry alerts by severity:

- **Critical (immediate notification):** Any error in payout calculation, balance mutation, or RNG generation. Any Stripe webhook failure. These need immediate attention.
- **High (within an hour):** API route 500 errors, authentication failures, database connection issues.
- **Medium (daily digest):** Client-side rendering errors, animation failures, non-critical UI bugs.

The goal is knowing about money-related problems in minutes, not hours. A payout bug running unchecked for an hour can cost thousands.

### Release Tracking

Sentry correlates errors with deploys. When you deploy a new version, Sentry tracks whether new errors appear. If payout errors spike after a deploy, you know exactly which release caused it and can roll back immediately.

This is especially important for a game. A SaaS app with a bug is embarrassing. A game with a payout bug is an emergency.

---

## Part 3: PostHog for Player Analytics

### What Analytics Means for a Game

Analytics answers: **How are players interacting with the game?**

For Frat House Frenzy, this means understanding:
- How players progress from signup to their first spin
- What bet sizes players prefer
- How often bonus rounds trigger in practice
- Whether players use the provably fair verification feature
- Where players drop off (deposit but never spin? spin but never deposit more?)
- How long play sessions last

### Event Tracking for a Slot Game

PostHog tracks events -- specific player actions. Each event has a name and properties:

- **Event:** `spin_completed`
  **Properties:** `bet_amount`, `win_amount`, `win_multiplier`, `is_bonus_spin`, `grid_size`, `cascade_count`

- **Event:** `bonus_triggered`
  **Properties:** `scatter_count`, `bonus_type` (Party/Darty/Full Send), `trigger_method` (natural/buy)

- **Event:** `deposit_completed`
  **Properties:** `amount`, `payment_method`, `is_first_deposit`

- **Event:** `withdrawal_requested`
  **Properties:** `amount`, `player_lifetime_deposits`, `player_lifetime_wins`

- **Event:** `provably_fair_verified`
  **Properties:** `spin_id`, `verification_result` (pass/fail)

- **Event:** `bonus_buy_clicked`
  **Properties:** `buy_tier` (80x/200x/500x/69x), `player_balance`

Events are the raw data. Everything else in PostHog -- funnels, trends, cohorts -- is built on top of events.

### Custom Game-Specific Metrics

Beyond standard events, track metrics unique to a slot game:

**Session metrics:**
- Average session duration
- Spins per session
- Total wagered per session
- Net result per session (win/loss)

**Feature engagement:**
- How many players edit their client seed (provably fair engagement)
- How many players use bonus buy vs. waiting for natural triggers
- Which bet amounts are most popular
- Average cascade depth (do cascades work as designed?)

**Financial metrics:**
- Gross Gaming Revenue (GGR) = total bets - total wins
- Average Revenue Per User (ARPU)
- Player lifetime value
- Deposit-to-first-spin time

### Direction for Claude Code

> "Set up PostHog event tracking for the game. Track these events: spin_completed (with bet, win amount, multiplier, cascade count), bonus_triggered (with scatter count, bonus type), deposit_completed (with amount, is_first_deposit), and provably_fair_verified (with result). Make sure events fire after the action completes successfully, not before."

---

## Part 4: Real-Time RTP Monitoring

### Why RTP Monitoring Is Critical

Your game targets 96.09% RTP. In theory, the math model guarantees this. In practice, bugs happen. A subtle error in the cascade payout logic could shift the RTP to 98%, quietly bleeding your bankroll. Or to 90%, quietly cheating players.

Real-time RTP monitoring tracks actual payouts vs. expected payouts and alerts you when something drifts.

### How It Works

Track two running totals:
1. **Total wagered** -- Sum of all bets placed
2. **Total paid out** -- Sum of all payouts (including bonus wins)

Calculate: **Actual RTP = total paid out / total wagered**

Over short periods (hundreds of spins), RTP will fluctuate wildly -- that's normal variance, especially with extreme volatility. Over longer periods (thousands of spins), it should converge toward 96.09%.

### Alert Thresholds

Set up alerts for when actual RTP deviates beyond expected variance:

- **Over 10,000 spins:** Alert if RTP is outside 93%-99% (wide band, high variance expected)
- **Over 100,000 spins:** Alert if RTP is outside 95%-97% (tighter band)
- **Over 1,000,000 spins:** Alert if RTP is outside 95.5%-96.7% (should be very close)

If you get an alert at the 1,000,000-spin level, something is almost certainly wrong in the math. Investigate immediately.

### PostHog Dashboard

Create a PostHog dashboard that shows:
- Rolling RTP over the last 1,000 / 10,000 / 100,000 spins
- Daily GGR trend
- Bonus trigger rate (actual vs. expected 1-in-195)
- Hit frequency (actual vs. expected 26.3%)
- Maximum win achieved to date

This dashboard is your cockpit. Check it daily. Any metric drifting from its expected value is an early warning sign.

---

## Part 5: Player Funnels

### The Player Journey

A funnel shows how players progress through a sequence of steps:

```
Signup → First Deposit → First Spin → 10th Spin → Bonus Round → Second Deposit
 100%      64%             58%          31%          12%            8%
```

This tells a story: 100 people sign up, but only 8 make a second deposit. Where are they dropping off?

- **Signup → First Deposit (36% drop)** -- Are players creating accounts but not depositing? Maybe the deposit flow is confusing, or the minimum deposit is too high.
- **First Spin → 10th Spin (27% drop)** -- Players try the game but don't stick. Is the base game engaging enough? Are wins too rare early on?
- **10th Spin → Bonus Round (19% drop)** -- Players leave before experiencing the bonus. The bonus trigger rate of 1-in-195 means many players won't see it in a short session.

Funnels turn guesses into data. Instead of "I think the onboarding is fine," you know "36% of signups never deposit."

### Key Funnels for Frat House Frenzy

1. **Acquisition funnel:** Landing page → Signup → Email verified → First deposit
2. **Activation funnel:** First deposit → First spin → First win → 10th spin
3. **Bonus funnel:** First spin → Bonus triggered → Bonus completed → Post-bonus deposit
4. **Retention funnel:** Day 1 active → Day 7 active → Day 30 active

---

## Part 6: Session Replay

### What It Is

Session replay records what players see and do. You can watch a recording of a player's session: every page they visited, every button they clicked, every spin result they saw, every time they hesitated.

### Why It Matters for a Game

Session replay answers questions that analytics can't:

- **"Why do players leave after 5 spins?"** Watch their sessions. Maybe they're confused by the cascade mechanic. Maybe the bet selector is hard to use on mobile. Maybe they hit a losing streak and the UI doesn't show enough feedback.
- **"Why don't players verify their spins?"** Watch sessions of players who won big. Did they notice the verification option? Is the button visible? Is the verification page confusing?
- **"Is the bonus round understandable?"** Watch a player experience their first bonus. Did they know what was happening? Did the UI explain the escalation system? Did they realize the grid was expanding?

### Debugging Player Reports

When a player reports "I won but didn't get paid," session replay is invaluable. You can watch exactly what happened: what they saw on screen, what the spin result showed, whether the balance updated. Combined with Sentry error tracking and the provably fair log, you can reconstruct the entire incident.

### Privacy Considerations

For a game with real money, be thoughtful about what you record:
- **Do record:** Game interactions, navigation, clicks, spin results, UI state
- **Don't record:** Payment form inputs (Stripe handles this securely), personal account details
- **Be transparent:** Tell players in your privacy policy that you use session replay for improving the game experience
- **Respect opt-out:** Give players the ability to disable session recording

---

## Part 7: Performance Monitoring

### Why Speed Matters for a Game

A slot game is an entertainment product. If the spin takes 2 seconds to process, the experience feels sluggish. If the cascade animation stutters, the excitement dies. Players expect near-instant feedback.

### What to Monitor

**Spin response time.** How long from the player clicking "Spin" to the server returning the result? This should be under 200ms. If it spikes, investigate -- maybe the payout calculation has a performance bug, or the database query is slow.

**Animation frame rate.** The reel spin, cascade, and win animations should run at 60fps. Dropped frames make the game feel cheap. Monitor frame rate in the browser and alert on sustained drops below 30fps.

**Time to interactive.** How long does the game take to load and become playable? Players who wait more than 3 seconds for a game to load will leave. Optimize the initial load: lazy-load sound files, defer non-critical assets, use Next.js Image optimization for symbol sprites.

**API error rate.** What percentage of spin requests result in errors? Even 0.1% is concerning at scale -- that's 1 error per 1,000 spins. Track and trend this over time.

### Lighthouse for the Game Page

Run Lighthouse audits on your game page. Target 90+ on Performance, Accessibility, and Best Practices. Pay special attention to:
- **LCP (Largest Contentful Paint):** The game grid should render quickly
- **INP (Interaction to Next Paint):** Clicking "Spin" should feel instant
- **CLS (Cumulative Layout Shift):** Nothing should jump around as assets load

---

## What's Next

Head to [project.md](project.md) to instrument Frat House Frenzy with Sentry and PostHog.
