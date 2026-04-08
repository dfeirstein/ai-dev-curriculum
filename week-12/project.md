# Project: Frat House Frenzy — Testing & Math Verification

**What you're building:** A comprehensive test suite for Frat House Frenzy -- unit tests for RNG fairness, payout accuracy, and provably fair verification, integration tests for the game engine, and E2E tests for the full spin flow.

**Time estimate:** 8-10 hours

**What you'll need open:** Ghostty (your terminal) and a web browser

---

## The Brief

Frat House Frenzy is a live game with real money on the line. A bug in the RNG means unfair outcomes. A bug in payout calculation means you're giving away money or shortchanging players. A bug in the provably fair system means the whole trust model collapses. This week you're adding the safety net: automated tests that prove the math is correct and the game engine works as designed.

You'll set up Vitest for unit and integration tests, write tests for every critical path in the game engine, and verify the math model through large-scale simulation. When you're done, running a single command will verify that Frat House Frenzy is fair, accurate, and functional.

---

## Phase 1: Research

Start Claude Code and research the testing approach:

> "Claude, I have a Next.js slot game (Frat House Frenzy) with TypeScript, Drizzle ORM, Better Auth, Stripe integration, a provably fair RNG system using HMAC-SHA256, and a math model targeting 96.09% RTP. I want to add comprehensive testing. What's the best way to set up Vitest for this project? How should I handle testing cryptographic functions, random number generation, and payout calculations?"

Follow up:

> "How do I write statistical tests for RNG fairness -- like a chi-squared distribution test over a large number of spins?"

> "What's the best approach for testing provably fair verification -- how do I test the full seed → result → verify chain?"

> "How do I simulate millions of spins efficiently in Vitest to verify the RTP converges to the expected value?"

---

## Phase 2: Plan

> "Claude, plan a comprehensive test suite for Frat House Frenzy. I want: (1) Vitest set up and configured for this project, (2) unit tests for RNG fairness using chi-squared distribution tests over 1M spins, (3) unit tests for payout calculation accuracy across all symbol combinations, (4) integration tests for the provably fair verification chain (server seed commit → client seed → HMAC-SHA256 → symbol mapping → verify), (5) E2E tests for the full spin flow (place bet → spin → cascade → payout → balance update), (6) a math model simulation that runs 10M spins and verifies RTP converges to 96.09%, (7) a /test skill that runs the full suite. Prioritize the tests by criticality -- what's most important to test first?"

Review the plan. Key questions:

- What test infrastructure is needed (test database, seed fixtures, etc.)?
- What are the critical paths that absolutely need tests?
- How will tests be organized in the project?
- How long will the 10M spin simulation take, and should it run separately from the fast tests?

---

## Phase 3: Execute

### Step 1: Set Up Vitest

> "Claude, set up Vitest for this project. Configure it to work with TypeScript, Next.js path aliases, and our existing project structure. Set up a test database (a separate Postgres database or an in-memory alternative) so tests don't affect development data. Add test scripts to package.json: 'test' for running all tests, 'test:unit' for unit tests only, 'test:integration' for integration tests only, 'test:math' for the long-running math model simulation. Create a test setup file that handles database seeding and cleanup."

Verify the setup:

> "Claude, create a simple smoke test -- just a test that asserts true equals true -- and run it with npm run test to make sure Vitest is working."

### Step 2: Unit Tests for RNG Fairness

> "Claude, write unit tests for the random number generator in Frat House Frenzy. Test: (1) HMAC-SHA256 output is deterministic -- same server seed, client seed, and nonce always produce the same result, (2) symbol distribution is uniform -- run 1,000,000 spins with random seeds and verify symbol frequencies match expected probabilities using a chi-squared test (p > 0.01), (3) no sequential correlation -- consecutive spins don't produce predictable patterns, (4) all reel positions are reachable -- every symbol can appear on every reel, (5) the mapping from HMAC output to reel symbols covers the full range without bias."

Run and verify:

> "Claude, run npm run test:unit and show me the results. Fix any failing tests."

### Step 3: Unit Tests for Payout Calculations

> "Claude, write unit tests for the payout calculation engine. For each symbol type, test: (1) minimum way win (3 of a kind on a 5x4 grid) pays the correct amount, (2) maximum way win (5 of a kind across all ways) pays the correct amount, (3) wild substitution works correctly -- The Keg substitutes for all non-special symbols, (4) xNudge Wild multiplier applies correctly -- +1 per nudge position, (5) cascading win payouts accumulate correctly across multiple cascades, (6) bet multiplier scales all payouts proportionally, (7) edge cases -- minimum bet ($0.10) and maximum bet ($100) calculate correctly."

Run and verify:

> "Claude, run npm run test:unit and show me the results."

### Step 4: Integration Tests for Provably Fair Verification

> "Claude, write integration tests for the provably fair system. Test the complete chain: (1) server generates a server seed and commits its SHA-256 hash before the spin, (2) player provides a client seed, (3) HMAC-SHA256(serverSeed, clientSeed:nonce) produces the game result, (4) the result maps to specific reel symbols deterministically, (5) after the spin, revealing the server seed allows independent verification -- hash the revealed seed and confirm it matches the pre-committed hash, (6) the same inputs always produce the same game outcome, (7) changing any input (server seed, client seed, or nonce) produces a different outcome. Test with known seed fixtures so results are predictable."

Run and verify:

> "Claude, run npm run test:integration and show me the results. Fix any failing tests."

### Step 5: Integration Tests for Game Engine

> "Claude, write integration tests for the game engine API routes. Test: (1) placing a bet deducts the correct amount from the player's balance, (2) a spin returns a valid reel layout (correct grid size, valid symbols only), (3) cascading wins trigger correctly -- winning symbols are removed and new symbols fall in, (4) the cascade chain terminates when no new wins form, (5) total payout from all cascades in a single spin is calculated correctly, (6) the player's balance is updated atomically -- no partial updates on failure, (7) a spin with insufficient balance is rejected, (8) bet amounts outside the valid range ($0.10-$100) are rejected."

Run and verify:

> "Claude, run npm run test:integration and show me the results."

### Step 6: Integration Tests for Bonus Features

> "Claude, write integration tests for the bonus round mechanics. Test: (1) 3 scatter symbols trigger Party Mode (8 free spins), (2) 4 scatters trigger Darty Mode (12 spins, 5x5 grid), (3) 5 scatters trigger Full Send Mode (12 spins, 5x6 grid, 3x starting multiplier), (4) the escalation system adds a row and +2 to the global multiplier on retrigger, (5) the Blackout Meter fills with cascades and triggers sticky-wild spins at 100%, (6) the Tip Jar Collector absorbs on-screen multipliers correctly, (7) bonus buy costs are correct (80x, 200x, 500x, 69x bet). Use deterministic seeds to force specific outcomes."

Run and verify:

> "Claude, run npm run test:integration and show me the results."

### Step 7: E2E Tests for the Full Spin Flow

> "Claude, use the Chrome tool to test the complete spin flow on the local dev server. Here's the flow: (1) Log in as a test player with a known balance, (2) Place a $1.00 bet, (3) Click spin, (4) Verify the reels display valid symbols, (5) If there's a win, verify the cascade animation triggers and new symbols fall in, (6) Verify the payout amount matches the expected calculation, (7) Verify the player's balance updates correctly (previous balance - bet + payout), (8) Verify the spin result appears in the game history, (9) Click the provably fair verification link and verify the result checks out."

> "Claude, use the Chrome tool to test the bonus buy flow. Log in, purchase a bonus round (80x buy), verify the correct amount is deducted, verify the bonus round starts with the correct mode, and verify free spins play out correctly."

> "Claude, use the Chrome tool to test error handling. Try to spin with zero balance, try to bet above the maximum, try to bet below the minimum. Verify each shows an appropriate error message."

### Step 8: Math Model Simulation

> "Claude, write a math model simulation test that runs 10,000,000 spins and verifies the game's statistical properties. The simulation should: (1) track total amount wagered and total amount paid out, (2) calculate the observed RTP and verify it's within 0.5% of the target 96.09%, (3) track hit frequency and verify it's close to the expected 26.3%, (4) track bonus trigger rate and verify it's close to 1 in 195 spins, (5) track the maximum win achieved and verify the game can produce high multipliers, (6) output a summary table with expected vs. observed values for each metric. This test should run with 'npm run test:math' and can take several minutes."

Run it:

> "Claude, run npm run test:math and show me the results. How close is the observed RTP to 96.09%?"

### Step 9: Create the /test Skill

> "Claude, create a /test skill in the CLAUDE.md file that runs the full quality pipeline in order: (1) npm run lint, (2) npm run typecheck, (3) npm run test (unit + integration, not the long math simulation), (4) npm run build. If any step fails, stop and report the failure. Show a summary at the end with pass/fail status for each step and the total test count."

Test the skill:

> "/test"

Verify it runs all four gates in order and reports results clearly.

### Step 10: Fix and Iterate

After running the full suite, there will likely be failures. This is normal and expected.

> "Claude, the RNG distribution test is failing -- the chi-squared statistic is too high. Check if the sample size is large enough, whether the expected distribution is calculated correctly, and whether there's actual bias in the symbol mapping function."

The key judgment: when a test fails, is the test wrong or is the code wrong? If the RNG is working correctly and the test threshold is too strict, adjust the test. If the symbol mapping has a bias the test exposed, fix the mapping. This is the value of testing.

### Step 11: Quality Gates and Deploy

> "Claude, run the full quality pipeline -- lint, typecheck, test, build. All gates must pass."

> "Claude, commit all changes with a clear message about the test suite, push to GitHub, and deploy."

---

## Acceptance Criteria

Your project is complete when all of these are true:

- [ ] Vitest is set up and configured for the project
- [ ] Unit tests verify RNG fairness via chi-squared distribution test over 1M spins
- [ ] Unit tests verify payout calculation accuracy for all symbol types and combinations
- [ ] Integration tests verify the provably fair chain (seed commit → HMAC → result → verify)
- [ ] Integration tests verify the game engine (bet → spin → cascade → payout → balance)
- [ ] Integration tests verify bonus round mechanics (triggers, escalation, Blackout Meter)
- [ ] E2E test of the full spin flow passes via Chrome tool
- [ ] E2E test of bonus buy flow passes via Chrome tool
- [ ] Math model simulation (10M spins) confirms RTP within 0.5% of 96.09%
- [ ] Math model simulation confirms hit frequency near 26.3%
- [ ] Math model simulation confirms bonus trigger rate near 1 in 195 spins
- [ ] A /test skill runs the full quality pipeline (lint, typecheck, test, build)
- [ ] All tests pass
- [ ] All quality gates pass (lint, typecheck, test, build)
- [ ] Code is on GitHub
- [ ] App is deployed and functional on Vercel

---

## Stretch Goals

**Test Coverage Report**
> "Claude, configure Vitest to generate a test coverage report. Show coverage percentages for each file and highlight any critical game engine files below 80% coverage. Add a coverage threshold to fail the test run if game engine, RNG, or payout files drop below 90%."

**Snapshot Tests for Game State**
> "Claude, add snapshot tests for the game state at each stage of a spin -- after reel stop, after each cascade, after payout. When the game state shape changes unexpectedly, the test should fail. This catches accidental breaking changes to the game engine contract."

**Stress Testing the Game Engine**
> "Claude, write a stress test that sends 100 concurrent spin requests from different players. Verify that all spins return correct results, no balances go negative, and no two spins share the same nonce. This tests whether the game handles concurrent play correctly."

**Edge Case Regression Tests**
> "Claude, write tests for known edge cases: the maximum win (42,069x) path, the Full Send Jackpot trigger conditions (15x+ global multiplier AND 100x+ Tip Jar in one spin), a spin that cascades more than 10 times, and a bonus round that retriggers to the maximum 5x7 grid. These are the scenarios most likely to have bugs."

---

## Troubleshooting

**Vitest won't run**
> "Claude, Vitest fails to start with this error: [paste error]. Check the Vitest configuration, TypeScript setup, and path alias configuration."

**Tests can't connect to the database**
> "Claude, integration tests fail with a database connection error. Check the test database configuration -- is the test database URL set in the test environment? Is the test database created and migrated?"

**RNG tests are flaky**
> "Claude, the chi-squared test passes sometimes and fails sometimes. This is a statistical test -- it has inherent variance. Check the sample size (should be at least 1M spins), the significance threshold (p > 0.01 is standard), and whether we're using a fixed seed for reproducibility or random seeds."

**Math model simulation is too slow**
> "Claude, the 10M spin simulation takes too long. Check if the game engine has unnecessary I/O in the simulation path. The simulation should run the pure math functions without database calls or network requests. Consider batching the random number generation."

**Provably fair tests fail with hash mismatch**
> "Claude, the provably fair verification test fails because the hash doesn't match. Check: (1) Is the server seed being hashed with SHA-256 before committing? (2) Is the HMAC using the exact format 'clientSeed:nonce'? (3) Are there encoding differences between the test and the implementation (UTF-8 vs. hex)?"

**Tests pass locally but build fails**
> "Claude, all tests pass when I run npm run test, but npm run build fails. Check for TypeScript errors in test files, import issues, or configuration problems that only surface during the build step."

---

## Reflection

You now have mathematical proof that your game is fair. Not "we think it's fair" -- a test suite that runs a million spins and verifies the distribution, checks every payout calculation, and proves the provably fair chain works end to end. That's what separates a legitimate game from a black box.

Notice the workflow: you described the statistical properties you wanted to verify, Claude Code wrote the tests and simulations, and you ran them to confirm the math checks out. You didn't write test code or implement chi-squared calculations. You thought about what "fair" means, what "correct" means, and what could go wrong. That's the skill -- the AI handles the implementation.
