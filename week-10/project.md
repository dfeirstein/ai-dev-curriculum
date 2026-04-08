# Project: Frat House Frenzy -- Game Engine Core

**What you're building:** The provably fair slot engine for Frat House Frenzy -- cryptographic RNG, symbol mapping, ways-to-win calculation, cascading/tumble mechanic, payout computation, and real-time win events. This is the math-heavy week.

**Time estimate:** 10-12 hours

**What you'll need open:** Ghostty (your terminal), a web browser, and a calculator (you'll be verifying math).

---

## The Brief

Frat House Frenzy has player accounts, balances, deposits, and bet controls. But when you hit "Spin," nothing actually happens. This week you build the brain of the slot machine: the game engine that generates provably fair random outcomes, maps them to symbols on a 5x4 grid, calculates wins using the ways-to-win system, handles cascading (tumble) mechanics where winning symbols disappear and new ones fall in, and computes payouts.

This is the most technically demanding week. The game engine is pure business logic -- no UI work, just math, cryptography, and API design. Every spin must be verifiable by the player using the provably fair system (HMAC-SHA256 with server and client seeds).

Think of this as building the core service that powers the game. The UI connects to it next week.

---

## Phase 1: Research

> "Claude, I'm building a provably fair slot game engine. The game uses a 5x4 grid with 1,024 ways to win (not paylines). I need to understand: how does ways-to-win calculation work? How is it different from traditional paylines? How do I calculate all winning combinations on a 5-reel, 4-row grid?"

> "Claude, explain the provably fair system used by crypto casinos. I need HMAC-SHA256 with a server seed (committed before the spin via SHA-256 hash), a client seed that the player can edit, and a nonce that increments with each spin. How do I map the HMAC output to symbol positions on a 5x4 grid?"

> "Claude, how does the cascading/tumble mechanic work in slots like Gates of Olympus or Sweet Bonanza? When symbols are part of a win, they're removed and new symbols fall in from above. Multiple cascades can happen in a single spin. How do I implement this with the provably fair system -- does each cascade use a new RNG call or is it all determined from the initial seed?"

> "Claude, I need to implement a specific math model. My slot has low-pay symbols (0.1x-0.4x for 3+ of a kind), high-pay symbols (1x-8x for 3+ of a kind), wilds, and special symbols. The target RTP is 96.09% with extreme volatility. How do I set up symbol weights to hit these targets? How do I verify the RTP through simulation?"

---

## Phase 2: Plan

> "Claude, plan the complete game engine for Frat House Frenzy. Here's the spec:
>
> **Provably Fair RNG:**
> - Server generates a random server seed and commits its SHA-256 hash before each game
> - Player has a client seed (default random, editable) and a nonce (auto-incrementing)
> - Each spin: HMAC-SHA256(serverSeed, clientSeed:nonce) produces a hex string
> - The hex string is mapped to symbol positions on the grid
> - After the game, the server seed is revealed so the player can verify
>
> **Symbol System:**
> - Low pays: Red Cup (0.1x/0.15x/0.4x for 3/4/5 of a kind), Blue Cup (0.15x/0.2x/0.5x), White Cup (0.15x/0.25x/0.6x), Green Cup (0.2x/0.3x/0.8x)
> - High pays: The Pledge (1x/1.25x/1.5x), The DJ (1.5x/1.75x/2x), Pong King (2x/2.5x/3x), House Mom (3x/4x/5x), Frat President (5x/6.5x/8x)
> - Wild (The Keg): Substitutes for all except scatter and special symbols
> - Scatter (Noise Complaint): 3/4/5 triggers bonus modes
> - Each symbol has a weight that determines how often it appears
>
> **Ways to Win (1,024 ways on 5x4):**
> - A win requires matching symbols on consecutive reels starting from reel 1
> - Each reel can have multiple matching symbols, and each combination is a separate way
> - Calculate all ways simultaneously, sum the payouts
>
> **Cascading/Tumble Mechanic:**
> - After a win, winning symbols are removed
> - Remaining symbols fall down, new symbols fill from the top
> - New symbols are determined from the same provably fair seed (use subsequent bytes)
> - Check for new wins, repeat until no more wins
> - Track the cascade chain length for the response
>
> **Payout Calculation:**
> - For each winning way: payout = bet * symbolPaytable[symbol][count] / totalWays
> - Wait, actually for ways games: payout = bet * symbolPaytable[symbol][count] * waysCount
> - Sum all payouts across all winning ways and all cascades
> - Multiply by any active multipliers
>
> **API Design:**
> - POST /api/game/spin: accepts betAmount, returns grid, wins, cascades, payout, seeds
> - GET /api/game/verify: accepts serverSeed, clientSeed, nonce -- returns the calculated grid so players can verify
> - POST /api/game/seed: lets the player update their client seed
> - GET /api/game/fairness: returns current server seed hash (commitment) and client seed
>
> **Win Events:**
> - After each spin, emit win data that the frontend can consume
> - Use Server-Sent Events (SSE) for real-time win notifications
> - Include: win amount, cascade chain length, biggest single win, total multiplier
>
> Plan the module structure, the RNG pipeline, symbol weight tables, payout tables, and the cascade loop algorithm."

Review the plan carefully. Key questions:

- Is the HMAC output long enough to fill a 5x4 grid (20 positions)?
- How are symbol weights applied to the RNG output?
- Does the cascade mechanic reuse the same seed or need additional randomness?
- How is the RTP verified before going live?
- Are all payout calculations using integer cents?

---

## Phase 3: Execute

### Step 1: Provably Fair RNG Module

> "Claude, build the provably fair RNG module for Frat House Frenzy:
> 1. A function to generate a random server seed using Node.js crypto.randomBytes(32)
> 2. A function to compute the SHA-256 hash of a server seed (the commitment)
> 3. A function to compute HMAC-SHA256(serverSeed, clientSeed:nonce) and return the hex string
> 4. A function to extract N random numbers from the HMAC hex output, each in a range [0, max). Use 4 bytes per number, convert to a value in range using modulo (or better, rejection sampling to avoid bias)
> 5. A verification function that takes serverSeed, clientSeed, nonce and reproduces the exact same output
> 6. Unit tests with Vitest: test that the same inputs always produce the same output, test that different inputs produce different output, test the range of extracted numbers
> Put this in a lib/rng.ts module."

Test:
- Run the unit tests
- Manually verify: compute HMAC-SHA256 of a known seed pair and check the output matches

> "Claude, show me the output of the RNG module for serverSeed='test-server-seed', clientSeed='test-client-seed', nonce=1. I want to verify it by hand."

### Step 2: Symbol System and Paytable

> "Claude, build the symbol system for Frat House Frenzy:
> 1. Define all symbols as a TypeScript enum or const object with IDs, names, and categories (low/high/special)
> 2. Define the paytable: for each symbol, the payout multiplier for 3, 4, and 5 of a kind
> 3. Define symbol weights for each reel position (reels can have different weight distributions)
> 4. Create a function that takes a random number and returns a symbol based on the weights (weighted random selection)
> 5. Create a function that maps an array of 20 random numbers to a 5x4 grid of symbols
> 6. Unit tests: verify weight distribution over 100,000 samples matches expected frequencies within 1%
>
> Use these payout values (multiplied by bet):
> - Red Cup: 0.1x / 0.15x / 0.4x (3/4/5 of a kind)
> - Blue Cup: 0.15x / 0.2x / 0.5x
> - White Cup: 0.15x / 0.25x / 0.6x
> - Green Cup: 0.2x / 0.3x / 0.8x
> - The Pledge: 1x / 1.25x / 1.5x
> - The DJ: 1.5x / 1.75x / 2x
> - Pong King: 2x / 2.5x / 3x
> - House Mom: 3x / 4x / 5x
> - Frat President: 5x / 6.5x / 8x
>
> Put this in lib/symbols.ts."

### Step 3: Ways-to-Win Calculator

> "Claude, build the ways-to-win calculator for the 5x4 grid:
> 1. A function that takes a 5x4 grid of symbols and returns all winning combinations
> 2. For each symbol type (excluding wilds and specials): check if it appears on reel 1, then reel 2, then reel 3, etc. Wilds count as matching any symbol
> 3. A win requires the symbol (or wild) on at least 3 consecutive reels starting from reel 1
> 4. For each winning symbol: count the number of ways (product of matching positions per reel) and the length (how many consecutive reels)
> 5. Return an array of win results: { symbol, count (reels), ways, payout }
> 6. Payout for each win = bet * paytable[symbol][count] * ways / totalWays... actually, check: in ways-to-win games, payout = bet / totalWays * paytable[symbol][count] * waysForThisWin. Verify the correct formula.
> 7. Unit tests: create a known grid with a known win and verify the calculator finds it with the correct payout
> Put this in lib/ways-calculator.ts."

Test thoroughly:

> "Claude, create a test grid where reel 1 has [FratPresident, Cup, Cup, Cup], reel 2 has [FratPresident, Wild, Cup, Cup], reel 3 has [FratPresident, Cup, Cup, Cup], reel 4 has [Cup, Cup, Cup, Cup], reel 5 has [Cup, Cup, Cup, Cup]. How many ways does the Frat President win on 3 reels? What's the payout at a $1.00 bet? Walk me through the math."

### Step 4: Cascade/Tumble Engine

> "Claude, build the cascade (tumble) mechanic:
> 1. After calculating wins on the initial grid, remove all symbols that were part of any winning combination
> 2. Remaining symbols fall down within their reel (gravity)
> 3. Empty positions at the top of each reel are filled with new symbols
> 4. New symbols come from the provably fair RNG -- use the next bytes from the same HMAC output (or extend the seed with an incrementing cascade counter: HMAC(serverSeed, clientSeed:nonce:cascadeN))
> 5. Check for new wins on the updated grid
> 6. Repeat until no more wins are found
> 7. Return the full cascade chain: an array of { grid, wins, payout } for each step
> 8. Total payout = sum of all cascade payouts
> 9. Unit tests: create a grid that should cascade and verify the chain
> Put this in lib/cascade-engine.ts."

### Step 5: Spin Orchestrator

> "Claude, build the spin orchestrator that ties everything together:
> 1. Accept a spin request: playerId, betAmount
> 2. Validate: player has sufficient balance, bet is within allowed range
> 3. Deduct the bet from balance (in a transaction)
> 4. Get or create the player's current server seed and client seed
> 5. Generate the initial grid using the RNG module
> 6. Run the ways-to-win calculator
> 7. If wins exist, run the cascade engine
> 8. Calculate total payout across all cascades
> 9. Credit any winnings to the player's balance
> 10. Record the spin in the database (bet, result grid, cascades, payout, seeds used)
> 11. Increment the nonce
> 12. Return: initial grid, cascade chain, total payout, win details, server seed hash (not the seed itself -- that's revealed later)
> Put this in lib/spin-orchestrator.ts."

### Step 6: Game API Routes

> "Claude, build the game API routes:
> 1. POST /api/game/spin -- calls the spin orchestrator, returns the full result
> 2. GET /api/game/verify -- accepts serverSeed, clientSeed, nonce in query params, reproduces the grid and wins, returns them for verification
> 3. POST /api/game/seed -- lets the player update their client seed (also rotates the server seed and reveals the old one)
> 4. GET /api/game/fairness -- returns the current server seed hash and the player's client seed
> 5. All routes protected by auth middleware
> 6. All inputs validated with Zod
> 7. Spin route must handle errors gracefully: if anything fails after the bet is deducted, refund the bet"

Test the API:
- Hit the spin endpoint with a valid bet -- verify you get a grid, wins, and payout
- Hit verify with the revealed server seed -- verify it produces the same grid
- Change the client seed and verify future spins use it
- Try spinning with insufficient balance -- should get a clear error

### Step 7: Win Event System

> "Claude, build a real-time win event system:
> 1. Server-Sent Events endpoint at /api/game/events that streams win notifications
> 2. When a spin produces a win, push a win event with: amount, multiplier, cascade chain length, biggest single cascade win
> 3. For big wins (50x+ bet), push a special 'big-win' event type
> 4. Include a recent wins feed: last 10 wins across all players (anonymized) for social proof
> 5. Client-side hook (useWinEvents) that connects to the SSE endpoint and provides the latest events
> 6. Fallback to polling every 5 seconds if SSE connection fails
> This maps to the notification system pattern from a SaaS app -- real-time events pushed to the client."

### Step 8: RTP Verification

> "Claude, build an RTP simulation tool:
> 1. A script at scripts/verify-rtp.ts that simulates 1,000,000 spins
> 2. Use a fixed bet amount ($1.00) and run through the full spin pipeline (RNG -> grid -> wins -> cascades -> payout)
> 3. Track: total wagered, total paid out, actual RTP (totalPaid / totalWagered * 100)
> 4. Track: hit frequency (% of spins that produce any win), average win size, max win
> 5. Print a summary report
> 6. The target RTP is 96.09% -- if the simulation is far off, we need to adjust symbol weights
> 7. Run it with: npx tsx scripts/verify-rtp.ts
> This is critical -- the math model must be verified before the game goes live."

Run the simulation:

> "Claude, run the RTP simulation and show me the results. If the RTP is not within 0.5% of 96.09%, adjust the symbol weights and run again."

### Step 9: Quality Gates

> "Claude, run npm run lint and fix any issues."

> "Claude, run npm run typecheck and fix any issues."

> "Claude, run npm run build and make sure it completes without errors."

### Step 10: Deploy

> "Claude, commit all changes, push to GitHub, and deploy to Vercel."

Test on the live URL:
- Hit the spin API endpoint and verify you get valid results
- Verify the provably fair system: spin, get the server seed hash, change client seed to reveal old server seed, verify the old spin
- Check that the SSE endpoint streams win events
- Verify the RTP simulation results are in the target range

---

## Acceptance Criteria

Your project is complete when all of these are true:

- [ ] Provably fair RNG module generates deterministic results from server seed + client seed + nonce
- [ ] SHA-256 commitment hash is provided before each spin
- [ ] Verification endpoint reproduces the exact same grid from revealed seeds
- [ ] Players can update their client seed (which rotates and reveals the old server seed)
- [ ] Symbol system uses weighted random selection with configurable weights per reel
- [ ] Paytable matches the design spec for all 9 regular symbols
- [ ] Ways-to-win calculator correctly identifies all winning combinations on a 5x4 grid
- [ ] Wilds substitute correctly in win calculations
- [ ] Cascade mechanic removes winning symbols, drops remaining, fills from top
- [ ] Cascades continue until no more wins, with provably fair new symbols
- [ ] Payout calculation is correct across all cascades
- [ ] Spin API deducts bet, runs engine, credits winnings, records the spin
- [ ] All balance mutations use database transactions with proper error handling
- [ ] SSE endpoint streams win events in real time
- [ ] RTP simulation runs and produces results within 0.5% of 96.09% target
- [ ] All game logic has unit tests
- [ ] `npm run lint` passes
- [ ] `npm run typecheck` passes
- [ ] `npm run build` completes without errors
- [ ] Code is on GitHub and deployed to Vercel

---

## Stretch Goals

**Add Scatter and Bonus Trigger Logic**
> "Claude, implement the scatter system. When 3/4/5 Noise Complaint scatters land, trigger the corresponding bonus mode (Party/Darty/Full Send). For now, just detect the trigger and return the bonus mode type in the spin result -- the actual bonus round gameplay will come later."

**Add the Mystery Symbol**
> "Claude, implement the Mystery Solo Cup. When mystery symbols land, they all reveal as the same random symbol (determined by the provably fair RNG). This can create unexpected big wins. Add this to the symbol system and cascade engine."

**Add Multiplier Tracking**
> "Claude, add a global multiplier that increases with each cascade step. Start at 1x, add 1x per cascade. All payouts in a cascade step are multiplied by the current multiplier. This dramatically increases volatility on long cascade chains."

**Add a Spin History API**
> "Claude, build a spin history endpoint that returns a player's last 50 spins with full details: grid, cascades, payout, and seeds. Include a 'Verify' button that checks each spin against the provably fair system. This is the transparency layer that builds player trust."

---

## Troubleshooting

**RTP simulation way off target**
> "Claude, the RTP simulation shows 89% instead of 96%. The symbol weights are wrong -- high-pay symbols are too rare or their payout multipliers are too low. Adjust the weights on each reel to increase the frequency of mid-range payouts. Re-run the simulation after each adjustment."

**Verification produces different results**
> "Claude, the verify endpoint produces a different grid than what was shown during the spin. Check: is the nonce being incremented at the right time (after the spin, not before)? Is the cascade using the correct extended seed? Are the same symbol weights used in both paths?"

**Cascades not working correctly**
> "Claude, symbols aren't falling down properly after a win. Check the cascade engine: after removing winning symbols, remaining symbols should shift DOWN within their column (reel), and new symbols fill from the TOP. Make sure the gravity function processes each reel independently."

**HMAC output too short for the grid**
> "Claude, the HMAC-SHA256 output is only 32 bytes but I need more random numbers for cascades. Use HMAC(serverSeed, clientSeed:nonce:0) for the initial grid, HMAC(serverSeed, clientSeed:nonce:1) for cascade 1, etc. Each cascade gets its own derivation."

**Floating point errors in payout calculation**
> "Claude, payouts are showing values like $1.9999999999 instead of $2.00. All payout calculations must use integer cents. Convert the paytable multipliers to integer operations: instead of 0.1x, use (bet * 10) / 100. Round at the end, not during intermediate steps."

---

## Reflection

This was the hardest week so far. You just built a cryptographically verifiable game engine -- the kind of system that online casinos spend months building with teams of mathematicians and engineers.

The provably fair system is particularly powerful. Players don't have to trust you -- they can verify every single spin. The server commits to the outcome before the spin (via the SHA-256 hash), the player contributes randomness (via their client seed), and after the game, everything is verifiable. This is the same commit-reveal pattern used in blockchain applications, voting systems, and cryptographic protocols.

The RTP simulation is how the industry actually works. Before a slot game goes live, the math model is simulated millions of times to verify it hits the target return. You did the same thing -- you didn't just build a game, you built the verification system that proves it's fair.

Next week, you'll connect this engine to a visual reel UI with animations, and ship v1.
