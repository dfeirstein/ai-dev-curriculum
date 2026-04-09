# Week 12 Guide: Frat House Frenzy — Testing & Math Verification

You shipped v1 of Frat House Frenzy. Players can sign up, deposit money, spin the reels, and (hopefully) win. Everything seems to work. Then you push an update -- maybe a small tweak to the cascade animation -- and suddenly the payout calculation is off by a factor of 10. A player hits a 500x win that should have been 50x. You don't notice until your balance sheet is wrecked.

This is what testing prevents. But for a slot game, the stakes are higher than a typical web app. A broken signup form in a SaaS tool is annoying. A broken payout function in a game that handles real money is catastrophic -- you're either giving away money you shouldn't or cheating players out of money they earned. Testing isn't optional here. It's survival.

---

## Part 1: Why Testing Matters More for a Game

### Money Is on the Line

In a SaaS app like a task manager, a bug means someone can't create a task. Frustrating, but nobody loses money. In Frat House Frenzy, a bug in the game engine means one of two things:

1. **You overpay.** A payout calculation error means players receive more than they should. At scale, this bleeds your bankroll dry.
2. **You underpay.** Players don't get what they earned. This is fraud, even if accidental.

Both are unacceptable. Both are preventable with proper testing.

### RNG Must Be Fair

Your game uses HMAC-SHA256 to generate random outcomes. If the randomness is biased -- if certain symbols appear more often than they should, or if the RNG has a pattern -- the entire game is compromised. Players will notice (or mathematicians will). Testing the randomness itself is a requirement, not a nice-to-have.

### The Math Model Must Be Verified

Frat House Frenzy has a target RTP of 96.09%. That number isn't aspirational -- it's a contract with your players. If your actual RTP drifts to 92% or 101%, something is wrong in the engine. The only way to verify RTP is to simulate millions of spins and check that the numbers converge. This is a kind of testing unique to games.

### What Tests Give You

**Confidence to change things.** The game engine is complex -- cascading wins, xNudge wilds, the Blackout Meter, the Tip Jar Collector. Changing one feature can break another. When your tests pass, you know the math still works.

**Proof of fairness.** A provably fair system means nothing if you can't prove it works. Tests are that proof.

**Faster feedback.** Running a million-spin simulation takes seconds. Manually verifying payout tables by hand takes forever and misses edge cases.

---

## Part 2: Unit Testing with Vitest

### What Unit Tests Are

Unit tests test individual functions in isolation. One function, one test. Does this specific piece of logic work correctly?

**Vitest** is the test runner you'll use. It's fast, works natively with TypeScript, and integrates well with Next.js. You don't need to understand Vitest's internals -- Claude Code handles the setup and syntax. You describe what to test.

### What to Unit Test in Frat House Frenzy

**Payout calculation.** Given a set of symbols on a 5x4 grid, does the payout function return the correct amount? This is the most critical function in the entire application.

Example scenarios:
- Five Frat President symbols across all reels = maximum base pay (8x per way)
- Three Red Solo Cups on the first three reels = minimum pay (0.1x)
- No matching symbols = zero payout
- A Keg wild substituting for a character symbol = correct substitution pay

**Symbol mapping.** Given an RNG output (the HMAC-SHA256 result), does the symbol mapping function produce valid symbols? Does every possible output map to a real symbol? Are there gaps or overlaps in the mapping ranges?

**Ways-to-win calculation.** The base game is 5x4 (1,024 ways). During features, the grid expands to 5x5, 5x6, or 5x7. Does the ways calculator return the right number for each grid size? Does it correctly count matching symbol paths across the reels?

**Multiplier logic.** Does the xNudge wild correctly add +1 to the multiplier per nudge? Does the Tip Jar Collector correctly sum all visible multiplier values? Does the global escalation multiplier cap at the right level?

**Bet validation.** Does the Zod schema reject bets below $0.10 or above $100? Does it reject non-numeric input? Negative numbers? Bets with more than two decimal places?

### Telling Claude Code What to Test

**Good direction:**
> "Write unit tests for the payout calculation function. Test every symbol at its minimum and maximum pay values. Test wild substitution. Test that zero matching symbols returns zero. Test edge cases: what happens with an all-wild row? What about the Mystery Solo Cup resolving to each possible symbol?"

**Not helpful:**
> "Write some tests for the game engine."

The first tells Claude Code exactly what behavior to verify. The second is too vague to produce useful tests.

---

## Part 3: Testing Randomness

### Why Randomness Needs Testing

Your HMAC-SHA256 RNG produces the outcomes for every spin. If the distribution is biased, the game is broken. But randomness is tricky to test -- by definition, random output looks irregular. How do you test that something is "random enough"?

### Chi-Squared Tests

A chi-squared test compares observed frequencies against expected frequencies. If your symbol mapping says the Red Solo Cup should appear 15% of the time, run 100,000 spins and check that it appears close to 15,000 times.

The chi-squared statistic tells you whether the deviation from expected is within normal random variation or suspiciously large. A high chi-squared value means the distribution is probably biased. A low value means it looks fair.

You don't need to understand the math behind chi-squared. You need to know what it does: it tests whether your RNG output matches the expected distribution.

**Direction for Claude Code:**
> "Write a test that generates 100,000 random symbol outcomes using our RNG function. Calculate the frequency of each symbol. Run a chi-squared test comparing observed frequencies to the expected frequencies from our symbol weight table. The test should fail if the p-value is below 0.01."

### Distribution Verification Over Large Samples

Beyond chi-squared, you want to verify specific distribution properties:
- Every symbol appears at least once in a large sample (no dead symbols)
- No symbol appears dramatically more or less than expected
- The distribution is consistent across different seed values
- Sequential outputs don't show obvious patterns (no runs of identical results)

These aren't exotic statistical tests. They're basic sanity checks that catch implementation bugs in the RNG pipeline.

---

## Part 4: Integration Testing

### Testing the Full Spin Flow

A unit test verifies that the payout function works. An integration test verifies that the entire spin flow works end-to-end:

1. Player places a bet ($1.00)
2. Server deducts $1.00 from player balance
3. RNG generates the outcome (server seed + client seed + nonce)
4. Symbol mapping converts RNG output to grid symbols
5. Payout calculation determines the win amount
6. Player balance is credited with the win
7. Spin result is recorded in the database
8. Response is sent to the client with symbols, payout, and new balance

An integration test sends a real HTTP request to the spin API route, checks that the database balance changes correctly, and verifies the response contains valid data.

**Example scenarios:**
- A spin with a known seed that produces a winning outcome -- verify the payout matches the expected amount
- A spin that results in no win -- verify balance decreases by the bet amount and no payout is credited
- A spin with insufficient balance -- verify it returns an error and doesn't deduct anything
- A spin with an invalid bet amount -- verify Zod validation rejects it
- A cascade sequence -- verify each cascade is processed and cumulative payouts are correct

### Testing the Provably Fair Chain

The provably fair system has three steps, and each must be tested:

1. **Seed commitment.** Before the spin, the server commits a hash of the server seed. Test that the commitment is a valid SHA-256 hash and that it's stored before the spin executes.

2. **Spin execution.** The spin uses HMAC-SHA256(serverSeed, clientSeed:nonce) to generate the outcome. Test that the same inputs always produce the same output (deterministic). Test that different inputs produce different outputs.

3. **Verification.** After the spin, the player can verify by hashing the revealed server seed and comparing to the pre-spin commitment. Test that the verification function correctly identifies valid spins (hash matches) and rejects tampered spins (hash doesn't match).

**Direction for Claude Code:**
> "Write integration tests for the provably fair system. Test the complete cycle: commit a server seed hash, execute a spin with known seeds, then verify the result. Test that verification passes with correct seeds and fails with tampered seeds. Test that the same server seed + client seed + nonce always produces the same outcome."

---

## Part 5: Math Model Simulation

### What a Simulation Is

A math model simulation runs millions of virtual spins and tracks the results. It doesn't test individual functions -- it tests the entire math model as a system. The question it answers: does the game's actual RTP converge to the target of 96.09%?

### How It Works

1. Set a starting bankroll (say, $1,000,000)
2. Set a fixed bet size ($1.00)
3. Run 1,000,000 spins
4. Track total wagered and total paid out
5. Calculate RTP = total paid out / total wagered
6. Verify it's within acceptable range of 96.09%

With 1 million spins, you'd expect the RTP to be within about ±0.5% of the target. With 10 million spins, within ±0.15%. The more spins, the tighter the convergence.

### What Else to Verify

Beyond aggregate RTP, the simulation should track:
- **Hit frequency:** What percentage of spins produce any win? Target: 26.3%
- **Bonus trigger rate:** How often do 3+ scatters appear? Target: ~1 in 195 spins
- **Maximum win achieved:** Did any spin approach the 42,069x cap?
- **Volatility distribution:** What percentage of wins are small (under 2x), medium (2x-50x), large (50x-500x), and massive (500x+)?
- **Feature frequency:** How often does Beer Pong Respin trigger? Party Foul? xWays Funnels?

If any of these metrics are significantly off, there's a bug in the game engine -- even if the aggregate RTP looks correct. A game could hit 96% RTP by paying out rarely but hugely, which would mean the hit frequency is wrong even though the math "works."

### Direction for Claude Code

> "Create a math model simulation that runs 1 million spins of the base game. Track total wagered, total paid, RTP, hit frequency, bonus trigger rate, and the distribution of win sizes. Assert that RTP is within 0.5% of 96.09%, hit frequency is within 2% of 26.3%, and bonus trigger rate is within 20% of 1-in-195. Make this runnable as a test with `npm run test:simulation`."

---

## Part 6: End-to-End Testing

### The Full Player Journey

E2E tests verify what actual players experience. They open a real browser, navigate the game, and perform actions.

**Critical player journeys to test:**
1. **Signup → Deposit → First Spin.** Create an account, add funds via Stripe test mode, place a bet, spin the reels, see a result.
2. **Win → Verify.** Hit a winning spin, check that the balance updates, then navigate to the verification page and confirm the provably fair hash matches.
3. **Bonus Round Entry.** Trigger a bonus (using a known seed that produces 3+ scatters), verify the bonus mode activates, play through free spins, verify the total bonus payout.
4. **Insufficient Balance.** Try to spin with zero balance, verify the game shows an appropriate error and doesn't let the spin execute.

### Using the Chrome Tool

The Claude Chrome tool is especially powerful for testing a game. Instead of writing test scripts in a framework, you describe the player journey:

> "Claude, use the Chrome tool to navigate to the game, log in with a test account, set the bet to $1.00, click Spin, wait for the animation to complete, and verify that the balance changed by the correct amount. Then click the 'Verify' button on the last spin and confirm the provably fair check passes."

Claude opens a real browser, performs the actions, and reports whether everything worked. This catches issues that unit and integration tests miss: broken animations, UI elements that don't update, buttons that don't respond.

---

## Part 7: The Quality Pipeline

### The Pipeline for Frat House Frenzy

The quality pipeline is a sequence of automated checks that your code must pass:

1. **Lint** (`npm run lint`) -- Code style and common mistakes
2. **Type-check** (`npm run typecheck`) -- TypeScript structural correctness
3. **Unit tests** (`npm run test`) -- Individual function behavior
4. **Integration tests** (`npm run test:integration`) -- Spin flow, provably fair chain
5. **Math simulation** (`npm run test:simulation`) -- RTP and distribution verification
6. **Build** (`npm run build`) -- Compilation check

All gates must pass. A game with correct logic but wrong math is still broken. A game with correct math but type errors will crash in production.

### The /test Skill

You'll create a Claude Code skill that runs the full pipeline with a single command. Instead of remembering which commands to run, you type `/test` and everything executes in order:

> "Claude, create a /test skill that runs lint, typecheck, unit tests, integration tests, the math simulation, and build -- in that order. Stop on the first failure and report what went wrong."

---

## Part 8: What to Test vs. What Not to Test

### Test These Things

**Payout calculations.** Every symbol combination, every multiplier interaction, every edge case. This is where bugs cost real money.

**The RNG pipeline.** Seed generation, HMAC computation, symbol mapping, distribution fairness.

**The provably fair system.** Commitment, execution, verification. This is your trust contract with players.

**Balance mutations.** Every operation that changes a player's balance: bets, wins, deposits, withdrawals. Test that balances never go negative, that wins are credited exactly once, that failed spins don't deduct.

**Validation logic.** Every Zod schema: bet amounts, deposit amounts, user inputs. Bad data must be rejected before it reaches the game engine.

**Bonus mechanics.** Scatter counting, free spin allocation, grid expansion, escalation system, Blackout Meter progression, Tip Jar collection.

### Don't Test These Things

**Third-party libraries.** Don't test that Stripe processes payments correctly. Stripe tests that. Test that your integration with Stripe works.

**Animation rendering.** Don't test that Framer Motion animates correctly. Test that the data driving the animation is correct.

**Framework behavior.** Don't test that Next.js routes requests. Test that your route handlers process spin requests correctly.

**Trivial code.** Don't test a function that returns a constant. Test functions that make decisions about money.

### The 80/20 Rule for Games

You don't need 100% test coverage. You need 100% coverage on the code that touches money. That means: payout calculation, balance mutations, RNG, and the provably fair chain. Everything else follows the normal 80/20 rule -- cover the critical paths, skip the trivial ones.

---

## What's Next

Head to [project.md](project.md) to add automated testing and math verification to Frat House Frenzy.
