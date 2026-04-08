# Project: Frat House Frenzy — Monitoring & Analytics

**What you're building:** Comprehensive monitoring and analytics for Frat House Frenzy -- Sentry for error tracking (especially game engine and payment failures) and PostHog for player analytics, behavior funnels, and real-time RTP monitoring.

**Time estimate:** 7-9 hours

**What you'll need open:** Ghostty (your terminal) and a web browser

---

## The Brief

Frat House Frenzy is live and tested. Now you need to see what's happening in production. A slot game has unique monitoring needs: you need to know instantly if the RTP drifts from target, if the game engine throws errors during a bonus round, or if a payment fails mid-deposit. This week you'll add two layers of visibility: Sentry catches errors before players report them, and PostHog shows you how the game is actually being played -- session lengths, betting patterns, bonus trigger rates, and retention. By the end, you'll have a real-time view into your game's health and player behavior.

---

## Phase 1: Research

Start Claude Code and research the monitoring approach:

> "Claude, I have a Next.js slot game (Frat House Frenzy) deployed on Vercel. I want to add Sentry for error monitoring with source maps, release tracking, and custom error boundaries -- especially for game engine errors and Stripe payment failures. I also want to add PostHog for player analytics: session length, average bet size, bonus trigger rates, RTP tracking, and player behavior funnels. What's the best integration approach for a Next.js App Router project?"

Follow up:

> "How do Sentry source maps work with Vercel deployments? Does Vercel handle source map uploading automatically?"

> "How should I structure PostHog event tracking for a slot game -- should game events fire from client components, server components, or API routes?"

> "What's the performance impact of adding Sentry and PostHog? Will it slow down spin response time or animation frame rate?"

---

## Phase 2: Plan

> "Claude, plan the complete monitoring and analytics integration for Frat House Frenzy. I need: (1) Sentry error monitoring with source maps, release tracking, custom error boundaries, and alert rules -- especially for game engine errors and payment failures, (2) PostHog event tracking based on this tracking plan: spin_completed, bonus_triggered, bonus_completed, deposit_completed, withdrawal_requested, big_win (50x+), jackpot_triggered, provably_fair_verified, session_started, session_ended, bet_size_changed, balance_low_warning, (3) custom PostHog metrics for real-time RTP tracking across all players with alerting if it deviates beyond acceptable range, (4) player behavior funnels, (5) PostHog session replay for debugging. Plan the integration order and any configuration needed."

Review the plan. Key questions:

- How does Sentry handle errors in both client and server components in Next.js?
- Where does each PostHog event get fired (client vs. server)?
- How do we calculate and track RTP in real-time across all players?
- What environment variables are needed for both services?

---

## Phase 3: Execute

### Step 1: Sentry Error Monitoring

> "Claude, add Sentry error monitoring to Frat House Frenzy. Use the official Sentry Next.js SDK. Configure it for: (1) automatic error capture in both client and server components, (2) source map uploading so production errors show original file names and line numbers, (3) user identification so errors include which player was affected -- use the authenticated user's ID and email, (4) release tracking tied to Git commits so we can correlate errors with deploys, (5) custom tags for game context: 'game_state' (base_game, bonus_round, jackpot), 'bet_amount', and 'balance' so we can see what was happening when an error occurred. Add the SENTRY_DSN and SENTRY_AUTH_TOKEN to the environment variables."

Test it:

> "Claude, add a temporary test button on the game page (only visible in development mode) that throws an intentional error during a simulated spin. I'll click it and verify it appears in Sentry."

Click the test button, then check your Sentry dashboard:
- Does the error appear?
- Does it show the correct source file and line number (not minified code)?
- Does it show which player triggered it?
- Does it include the game state tags?
- Does it show breadcrumbs leading up to the error?

> "Claude, remove the test error button now that Sentry is verified."

### Step 2: Custom Error Boundaries for Game States

> "Claude, add custom error boundaries to Frat House Frenzy that report to Sentry. Create error boundaries for: (1) the root layout -- catch any top-level render error and show a friendly 'Something went wrong' page with a retry button, (2) the game engine -- if the spin calculation fails, preserve the player's balance and show an error state without losing their bet, (3) the bonus round -- if the bonus round errors mid-way, save the current state and allow resumption, (4) the payment flow -- if a deposit or withdrawal fails, show clear status and next steps. Each error boundary should report the error to Sentry with context about which boundary caught it and what game state the player was in."

Test it:

> "Claude, temporarily introduce an error in the game engine to verify the error boundary catches it, preserves the player's balance, and shows the friendly error state instead of a crash."

Verify, then fix the intentional error.

### Step 3: Sentry Alerts for Game-Critical Errors

Configure alerts in the Sentry dashboard (this is done in the Sentry web UI, not in code):

1. Go to your Sentry project > Alerts > Create Alert Rule
2. Create a "Game Engine Error" alert: notify immediately when any error occurs in the game engine or payout calculation code
3. Create a "Payment Failure" alert: notify when Stripe payment errors occur
4. Create a "High Frequency" alert: notify when any error occurs more than 10 times in one hour

> "Claude, what alert rules should I set up in Sentry for a production slot game? Walk me through the configuration in the Sentry dashboard. I need to catch game engine errors, payment failures, and provably fair verification failures immediately."

### Step 4: PostHog Player Analytics

> "Claude, add PostHog analytics to Frat House Frenzy. Initialize the PostHog client in the app layout. Implement the following tracking plan -- fire each event at the appropriate point in the code:

Events to track:
- spin_completed: after every spin, with properties 'bet_amount', 'payout', 'multiplier', 'is_win', 'cascade_count', 'game_mode' (base/bonus)
- bonus_triggered: when scatters trigger a bonus, with properties 'scatter_count', 'bonus_type' (party/darty/full_send), 'trigger_spin_number'
- bonus_completed: when a bonus round ends, with properties 'bonus_type', 'total_payout', 'spins_played', 'max_multiplier'
- big_win: when a win exceeds 50x bet, with properties 'multiplier', 'payout', 'game_mode'
- jackpot_triggered: when Full Send Jackpot activates, with properties 'global_multiplier', 'tip_jar_value'
- deposit_completed: after successful deposit, with properties 'amount', 'method'
- withdrawal_requested: after withdrawal request, with properties 'amount'
- bet_size_changed: when player adjusts bet, with properties 'old_bet', 'new_bet'
- provably_fair_verified: when a player checks verification, with properties 'result_matched'
- session_started: when a player opens the game
- session_ended: when a player leaves, with properties 'session_duration', 'total_spins', 'net_result'

Identify players with their user ID and email after login."

Test it:
- Play several spins, trigger a bonus if possible, change bet size, check provably fair
- Go to your PostHog dashboard
- Verify each event appears with the correct properties

### Step 5: Real-Time RTP Monitoring

> "Claude, implement real-time RTP tracking in PostHog. Create a server-side function that: (1) after every spin, updates a running total of amount_wagered and amount_paid_out in the database, (2) calculates the current observed RTP (total_paid / total_wagered * 100), (3) sends a custom PostHog event 'rtp_checkpoint' every 1,000 spins with properties 'observed_rtp', 'total_spins', 'total_wagered', 'total_paid', (4) fires an alert event 'rtp_deviation_warning' if the observed RTP deviates more than 2% from the target 96.09% after at least 100,000 spins. Also create a PostHog dashboard insight that shows the RTP trend over time as a line chart."

> "Claude, set up a PostHog action that triggers a webhook notification when the 'rtp_deviation_warning' event fires. Walk me through the PostHog dashboard configuration."

### Step 6: Player Behavior Funnels

> "Claude, help me set up player behavior funnels in PostHog for Frat House Frenzy:

Funnel 1 — New Player Journey:
signup_completed → deposit_completed → spin_completed → bonus_triggered → session_ended (with net positive result)

Funnel 2 — Retention Funnel:
session_started (day 1) → session_started (day 2) → session_started (day 7) → deposit_completed (repeat deposit)

Funnel 3 — High Roller Path:
bet_size_changed (to max) → big_win → jackpot_triggered

Walk me through creating these funnels in the PostHog dashboard. What does each funnel tell us about player behavior?"

### Step 7: Performance Monitoring

> "Claude, add performance monitoring for the game's critical paths: (1) measure spin response time (from bet placement to result return) and send as a PostHog event 'spin_performance' with 'response_time_ms', (2) measure animation frame rate during cascades and big win celebrations using requestAnimationFrame, send 'animation_performance' with 'avg_fps' and 'dropped_frames', (3) measure time-to-interactive for the game page load. Set up PostHog insights to track these metrics over time. Alert if spin response time exceeds 500ms or animation FPS drops below 30."

### Step 8: PostHog Session Replay

> "Claude, enable PostHog session replay for Frat House Frenzy. Configure it to: (1) record sessions for all players, (2) mask sensitive input fields (passwords, payment info, seed values), (3) capture console logs and network requests for debugging context. Set the sample rate to 100% for now (we'll reduce it when we have more players)."

Test it:
- Play through several spins, trigger some events
- Go to PostHog > Session Replay
- Find your session and play it back
- Verify it recorded the gameplay accurately
- Verify sensitive fields are masked

### Step 9: Verify the Complete Picture

Now step back and verify that the monitoring and analytics stack is working together:

> "Claude, use the Chrome tool to run through a complete player journey on the dev server: sign up, deposit funds, play 20 spins at different bet sizes, check provably fair verification for one spin, view game history. After the journey, I'll check both Sentry (no errors should have been captured) and PostHog (all events should appear in order with correct properties)."

Check Sentry:
- No new errors from the test journey (good -- the happy path works)
- The existing test errors from Step 1 are still there

Check PostHog:
- All events from the journey appear in chronological order
- Player identification is correct
- Event properties are populated
- RTP checkpoint event appears

### Step 10: Quality Gates and Deploy

> "Claude, run the full quality pipeline: lint, typecheck, test, build. Make sure the Sentry and PostHog integrations don't break any existing tests."

> "Claude, commit all changes with a clear message about monitoring and analytics, push to GitHub, and deploy."

After deployment, verify on the live URL:

> "Claude, add the Sentry and PostHog environment variables to the Vercel dashboard if they aren't there already. Walk me through the configuration."

Visit the production URL and play a few spins. Check:
- Sentry release tracking: does the new deployment appear as a release?
- PostHog events: are production events flowing?
- Session replay: does a production session get recorded?
- RTP tracking: is the running total updating?

---

## Acceptance Criteria

Your project is complete when all of these are true:

- [ ] Sentry is capturing errors with source maps (verified in Sentry dashboard)
- [ ] Sentry errors include player context (user ID, email, game state)
- [ ] Sentry release tracking is connected to deploys
- [ ] Custom error boundaries catch game engine and payment errors gracefully
- [ ] Sentry alert rules are configured (game engine errors, payment failures, high frequency)
- [ ] PostHog event tracking is implemented for all events in the tracking plan
- [ ] PostHog events include correct properties (verified in PostHog dashboard)
- [ ] PostHog player identification works (logged-in players are identified)
- [ ] Real-time RTP tracking calculates and reports observed RTP
- [ ] RTP deviation alerting is configured (warns if RTP drifts more than 2% after 100K spins)
- [ ] Player behavior funnels are set up in PostHog (new player, retention, high roller)
- [ ] Performance monitoring tracks spin response time and animation FPS
- [ ] PostHog session replay is recording sessions with sensitive data masked
- [ ] No new errors in Sentry from the monitoring/analytics integration itself
- [ ] All existing tests still pass
- [ ] All quality gates pass (lint, typecheck, test, build)
- [ ] Code is on GitHub
- [ ] App is deployed and functional on Vercel with monitoring active

---

## Stretch Goals

**Custom Sentry Dashboard for Game Health**
> "Claude, help me configure a custom Sentry dashboard that shows: game engine errors by game state (base vs. bonus), payment errors by type (deposit vs. withdrawal), error frequency over the last 7 days, and most affected players. Walk me through the Sentry dashboard configuration."

**PostHog Cohort Analysis**
> "Claude, help me set up player cohorts in PostHog: whales (top 10% by deposit amount), casual players (fewer than 10 spins per session), bonus hunters (high bonus buy rate), and churned players (no session in 7 days). Create insights comparing behavior across these cohorts."

**Analytics Opt-Out**
> "Claude, add an analytics opt-out option to Frat House Frenzy. In the player settings, add a toggle that lets players disable PostHog tracking and session replay. When opted out, no events should be sent. Store the preference and respect it across sessions."

**Custom Sentry Performance Monitoring**
> "Claude, enable Sentry performance monitoring for Frat House Frenzy. Track transaction durations for key operations: spin calculation, payout processing, provably fair verification, deposit flow, and withdrawal flow. Set up alerts for slow transactions."

---

## Troubleshooting

**Sentry errors show minified code instead of source files**
> "Claude, Sentry is showing minified file names and line numbers. Source maps aren't working. Check the Sentry source map configuration, the SENTRY_AUTH_TOKEN, and whether source maps are being uploaded during the build step."

**PostHog events aren't appearing**
> "Claude, I played several spins but PostHog shows no events. Check: (1) Is the NEXT_PUBLIC_POSTHOG_KEY correct? (2) Is PostHog initialized in the app layout? (3) Are events being fired from client components (PostHog client SDK runs in the browser)? (4) Is there an ad blocker interfering?"

**Session replay isn't recording**
> "Claude, PostHog session replay shows no sessions. Check the session replay configuration, the sample rate, and whether the recording script is loading on the page. Ad blockers can also block session recording."

**RTP tracking shows incorrect values**
> "Claude, the real-time RTP is showing [number]% which seems way off from 96.09%. Check: (1) Are we counting both wagered and paid amounts correctly? (2) Are bonus round payouts being included? (3) Is the sample size large enough (need at least 100K spins for the RTP to converge)? (4) Are we double-counting cascade payouts?"

**Tests fail after adding monitoring**
> "Claude, existing tests are failing after adding Sentry and PostHog. The test environment probably doesn't have the monitoring environment variables. Mock or disable Sentry and PostHog in the test environment so they don't interfere with tests."

---

## Reflection

You now have visibility into your production game. When something breaks, Sentry tells you -- with full context about what the player was doing, what game state they were in, and where in the code the problem lives. When you want to understand how people play, PostHog shows you -- which bet sizes are popular, where players drop off, how often bonuses trigger, and what the experience looks like through session replay.

Most importantly, you're monitoring the math. The real-time RTP tracker means you know your game is paying out fairly not because you assume it is, but because you have live data proving it. That's the difference between operating a game and just having a game deployed. Operating means you're watching, learning, and responding.
