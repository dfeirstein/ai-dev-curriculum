# Project: Speed Build — Second Game

**What you're building:** A complete second game, built and shipped in 5 days, reusing the engine and patterns from Frat House Frenzy.

**Time estimate:** 15-20 hours across the week (3-4 hours per day)

**What you'll need open:** Ghostty, your browser, a timer (seriously -- time-box yourself)

---

## The Brief

Pick a game. Theme it. Build it. Ship it. Five days.

You have a published game engine, a proven architecture, and a complete reference implementation in Frat House Frenzy. This week proves you can ship fast by reusing what you've already built. The engine handles the math. The provably fair system handles trust. The sound and animation patterns handle polish. Your job is to configure a new game on top of all of it.

This is not about building from scratch. It's about proving your architecture actually works for more than one product.

---

## Game Ideas

Pick one. Each can be built in 5 days because the hard infrastructure already exists.

1. **Simple Slot -- "Dorm Room Classic"** -- A 3x3, 5-line classic slot. No cascades, no expanding grid. Just spins, wilds, a free spin bonus, and a clean retro aesthetic. Proves the engine works for a minimal configuration.
2. **Crash Game -- "Send It"** -- A multiplier climbs from 1.00x upward. Cash out before it crashes. Provably fair (use the same HMAC-SHA256 system to determine the crash point). Real-time via WebSocket -- all players see the same round.
3. **Mines Game -- "Frat House Minefield"** -- A 5x5 grid with hidden mines. Pick tiles to reveal multipliers. Each safe pick increases the multiplier. Hit a mine, lose your bet. Provably fair (mine positions determined by hash).
4. **Dice Game -- "Beer Die"** -- Set an over/under target. Roll provably fair dice. Simple but needs a slick UI with roll animations and bet history. Add a streak tracker and multiplier betting.
5. **Plinko -- "Cup Pong Drop"** -- Drop a ball through a peg board. It bounces to a multiplier at the bottom. Provably fair (drop position + ball path determined by hash). Satisfying physics animation is the core challenge.

Pick something different enough from Frat House Frenzy that you're solving new problems, but similar enough that you can reuse the infrastructure.

---

## Day 1: Research + Scaffold

**Goal:** End the day with a working project skeleton -- auth, database, game page, and the engine package installed. No game logic yet.

### Morning: Choose and Plan (1-2 hours)

**Pick your game (15 minutes max):**
Pick from the list or design your own. Don't deliberate. Pick and go.

**Define the math model (30 minutes):**
> "I'm building a [game type] called [name] using my published game engine package. Define a math model for this game: target RTP of 96%, medium volatility. What are the possible outcomes, their probabilities, and their payouts? Show me the expected value calculation that proves the RTP."

**Plan the theme and assets (30 minutes):**
> "I'm theming this game as [theme]. What symbols/visuals do I need? List every visual asset, sound effect, and animation I'll need to build. Keep the list short -- this is a 5-day build."

### Afternoon: Scaffold (2-3 hours)

> "Create a new Next.js project called [game-name]. Set up TypeScript, Tailwind, shadcn/ui, Postgres with Drizzle, and Better Auth. Install my published game engine package. Create the basic layout with navigation, a game page, and an account page. Deploy the skeleton to Vercel."

**If building a slot game**, configure the engine:

> "Configure the game engine for a 3x3, 5-line classic slot. Define the symbol set: [list symbols and pay values]. Create the reel strips. Set up the provably fair system using the same HMAC-SHA256 flow from Frat House Frenzy."

**If building a crash/mines/dice/plinko game**, extend the engine:

> "The game engine's provably fair system generates a hash from HMAC-SHA256(serverSeed, clientSeed:nonce). I need to map that hash to a [crash point / mine layout / dice roll / plinko path]. Write a deterministic mapping function that converts the hash to the game result. The mapping must be verifiable -- given the same inputs, anyone can reproduce the same result."

**Day 1 Deliverable:** A deployed skeleton with auth, database, and the engine package installed. The math model is defined and the provably fair mapping is implemented.

---

## Day 2: Core Game Loop

**Goal:** The core game action works -- a player can place a bet and play a round.

### Build the Game Logic (2-3 hours)

> "Build the core game loop for [game name]. A player sets their bet amount, initiates a round, the server generates a provably fair result, and the client displays the outcome. Store each round in the database: player ID, bet amount, server seed hash (pre-reveal), client seed, nonce, result, and payout. Deduct the bet from the player's balance before the round and credit any winnings after."

### Build the Game UI (2-3 hours)

> "Build the game interface. [Describe the specific UI for your chosen game -- reel grid for slots, climbing multiplier for crash, tile grid for mines, etc.]. Use Framer Motion for the core animation: [reel spin / multiplier climbing / tile reveal / dice roll / ball drop]. The animation should feel satisfying -- this is the core of the game experience. Add bet controls: bet amount input with preset buttons, a play/spin button, and the player's current balance."

### Deploy and Verify

Push to main. Play a round on the live site. Does the bet deduct? Does the result display? Does the payout credit?

**Day 2 Deliverable:** The core game works end-to-end. A player can bet and play.

---

## Day 3: Features and Polish

**Goal:** Add the features that make the game interesting, not just functional.

### Add Game-Specific Features (2-3 hours)

**For a slot game:**
> "Add a free spin bonus triggered by 3 scatter symbols. 10 free spins with a 2x multiplier on all wins. Add a wild symbol that substitutes for all symbols except scatter."

**For a crash game:**
> "Add auto-cashout (player sets a target multiplier, automatically cashes out if reached). Add a game history panel showing the last 20 crash points. Add a live player list showing who's in the current round and who has cashed out."

**For a mines game:**
> "Add difficulty selection (easy: 3 mines, medium: 5 mines, hard: 10 mines, expert: 15 mines). Add an auto-reveal button that picks a random safe tile. Add a cashout button that locks in current winnings at any point."

**For a dice game:**
> "Add configurable over/under targets with dynamic payout calculation. Add a streak tracker that shows consecutive wins. Add auto-bet with configurable stop conditions (stop on loss, stop on profit target)."

**For plinko:**
> "Add multiple risk levels (low/medium/high) that change the multiplier distribution at the bottom. Add a ball trail effect. Add a history panel showing the last 20 drops and their results."

### Sound and Visual Polish (1-2 hours)

> "Add Howler.js sounds for the core game actions: [list specific sounds for your game]. Add a background ambient track. Add a win celebration for big wins. Reuse the tiered win presentation pattern from Frat House Frenzy but adapt the visuals to this game's theme."

### Provably Fair Verification (30 minutes)

> "Add the provably fair verification component to this game. Configure it with this game's hash-to-result mapping function. Add a link to the verification page from the game history -- each past round should have a 'Verify' link that pre-fills the verification form."

**Day 3 Deliverable:** The game has meaningful features beyond the core loop. Sound and provably fair verification are in place.

---

## Day 4: Testing + Monitoring

**Goal:** Add testing, monitoring, and fix bugs.

### Testing (2-3 hours)

> "Add Vitest to this project. Write unit tests for: the hash-to-result mapping function (deterministic output for known inputs), the payout calculation (correct amounts for all outcomes), the provably fair verification (verify a known round), and balance management (bet deduction, win credit, insufficient balance rejection). Aim for at least 15 test cases."

> "Set up Playwright and write one E2E test for the critical flow: sign up, place a bet, play a round, verify balance updates correctly."

> "Run a 50,000-round simulation to verify the RTP is within 0.5% of the target. Log the distribution of outcomes and compare to the math model."

### Monitoring (30 minutes)

> "Set up Sentry error monitoring. Set up PostHog analytics tracking: page views, rounds played, bet amounts, and big win events."

### Bug Fixes (remaining time)

Play the game for 20 minutes straight. Find and fix any bugs. Pay attention to: balance going negative, animations getting stuck, sounds overlapping, and the provably fair nonce incrementing correctly.

**Day 4 Deliverable:** Tests passing, monitoring active, bugs fixed. The game is reliable.

---

## Day 5: Ship It

**Goal:** Make it presentable and ship.

### Performance (30 minutes)

> "Run Lighthouse on the game page. Fix any issues to get all scores above 80."

### UI Polish (1 hour)

> "Add loading states, error states, and empty states. Do a mobile responsiveness pass -- the game should be playable on a phone. Add a responsive game layout that adapts the controls for touch input."

### README and Documentation (30 minutes)

> "Write a README for this project. Include: game description, live demo link, features list, tech stack (highlight that it uses the published game engine), how the provably fair system works, and local setup instructions."

### Final Deployment (15 minutes)

Push everything. Verify the live site one last time:

- [ ] All game features work on the live site
- [ ] Auth works (sign up, sign in, sign out)
- [ ] Bets deduct and wins credit correctly
- [ ] Provably fair verification works for past rounds
- [ ] Sounds play and can be muted
- [ ] No visible errors in the console
- [ ] README is accurate

**Day 5 Deliverable:** A complete, deployed, documented second game. Done.

---

## Acceptance Criteria

- [ ] **New game** (different type or significantly different mechanics from Frat House Frenzy)
- [ ] **Uses the published game engine** package (provably fair system, RNG)
- [ ] **Deployed to Vercel** with a live URL
- [ ] **Authentication** working
- [ ] **Core game loop** functional -- bet, play, payout
- [ ] **Game-specific features** beyond the basic loop (at least 3)
- [ ] **Provably fair verification** page with the reusable component
- [ ] **Sound design** with Howler.js (game SFX + background audio + mute control)
- [ ] **Database** tracking all rounds with full provably fair audit trail
- [ ] **Unit tests** covering core game logic and math model
- [ ] **E2E test** covering the critical play flow
- [ ] **RTP verified** by simulation (within 0.5% of target)
- [ ] **Sentry** error monitoring active
- [ ] **PostHog** analytics tracking key events
- [ ] **README** with description, demo link, and provably fair explanation
- [ ] **Completed in 5 days**

---

## Reflection

You just built a second game in a week. Compare that to the weeks it took to build Frat House Frenzy. The difference is architecture. When you extracted the engine, built reusable components, and established patterns, you made every future game faster to build.

This is the real payoff of good engineering: the first project is slow because you're building the foundation. The second project is fast because the foundation already exists. The third would be even faster. That's how professional teams work -- they invest in reusability and it compounds.

You now have two deployed, provably fair games, a published npm package, and a portfolio that demonstrates you can build, extract, reuse, and ship. That's not a student portfolio. That's an engineering portfolio.
