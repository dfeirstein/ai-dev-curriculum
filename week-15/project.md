# Project: Frat House Frenzy — Advanced Mechanics (Week 1/2)

**What you're building:** The advanced base game features for Frat House Frenzy -- xNudge Wilds, Beer Pong Respin, Party Foul events, xWays Funnels, and the animations that bring them to life.

**Time estimate:** 15-20 hours across the week

**What you'll need open:** Ghostty, your browser, the game design brief (`reference/game-design-brief.md`)

---

## The Brief

Your base game works. Players can spin, symbols cascade, and wins pay out with provably fair verification. But right now every spin feels the same. This week you add the mechanics that make Frat House Frenzy play like a real high-volatility slot -- wilds that nudge into view and stack multipliers, reels that lock and respin, random events that break the rhythm, and expanding ways that blow the grid open.

Each mechanic needs its own animation. A nudge wild sliding down a reel with a multiplier counter climbing. Locked reels shaking during a respin. Mystery cups flipping to reveal matching symbols. These animations are what make the difference between a math engine and a game people want to play.

---

## Day 1: xNudge Wild Mechanic

### Step 1: Understand the Mechanic

The xNudge Wild is The Keg Stand symbol. It's a full-reel wild -- it occupies every position on a reel. When it lands partially visible (only 1-3 positions showing), it nudges into full view. Each nudge adds +1 to a multiplier that applies to every win involving that wild.

Example: The Keg Stand lands showing 2 of 4 positions on reel 3. It nudges down 2 positions to fill the reel. The multiplier is now 3x (base 1x + 2 nudges). Every winning combination that passes through reel 3 gets multiplied by 3x.

### Step 2: Build the xNudge Engine Logic

> "I need to build the xNudge Wild mechanic for Frat House Frenzy. Here's how it works: The Keg Stand is a full-reel wild symbol. When it lands partially visible on any reel, it nudges into full view -- each position it nudges adds +1 to a multiplier (starting at 1x). The multiplier applies to all wins that include that reel. Build the engine logic first -- determine nudge count, calculate the multiplier, and integrate it with the existing win evaluation. Add Vitest tests that verify: a wild showing 1 of 4 positions gets 4x multiplier, a wild showing 3 of 4 gets 2x, and two xNudge wilds on different reels multiply together."

Verify the math by running the tests. The multiplier calculation is the foundation -- if it's wrong, everything built on top will be wrong.

### Step 3: Animate the Nudge

> "Add Framer Motion animations for the xNudge Wild. When The Keg Stand lands partially visible: first show the partial symbol, then animate it sliding into full view (one position at a time, 200ms per nudge). Display a multiplier counter next to the reel that increments with each nudge -- use a scale-up + glow effect on each increment. The final multiplier should pulse briefly before wins are evaluated."

Test this visually. Spin until you get an xNudge wild (or temporarily increase its frequency for testing). The nudge should feel satisfying -- a reel sliding into place with a multiplier climbing.

---

## Day 2: Beer Pong Respin

### Step 4: Build the Respin Engine Logic

The Beer Pong Respin triggers when two xNudge Wilds land on different reels. The reels between them lock, and all other reels respin. If the wilds have multipliers, those multipliers multiply together on the respin evaluation.

> "Build the Beer Pong Respin mechanic. Trigger condition: two xNudge Wilds land on different reels in the same spin. When triggered: lock the wild reels and all reels between them. Respin the remaining reels once. Evaluate wins using the locked wilds and their multipliers -- if both wilds have multipliers, multiply them together. Add this to the game engine with proper state management. Write tests for: trigger detection (wilds on reels 1 and 3, wilds on reels 2 and 5, single wild doesn't trigger), locked reel identification, and combined multiplier calculation."

### Step 5: Animate the Respin

> "Add animations for the Beer Pong Respin. When triggered: flash the two wild reels with a neon highlight, then show the reels between them locking into place (a padlock icon or chain effect). Dim the locked reels slightly. Spin only the unlocked reels with a distinct animation -- faster spin speed, different easing. When the respin completes, evaluate and show wins with the combined multiplier displayed prominently."

### Step 6: Test the Full Flow

Play through the xNudge + Beer Pong combination manually. Does a partial wild nudge into view, show its multiplier, then trigger a respin when a second wild appears? Does the combined multiplier feel impactful? Temporarily boost wild frequency to test this efficiently.

---

## Day 3: Party Foul Random Events

### Step 7: Build the Party Foul System

Party Foul triggers randomly, about 1 in 40 spins. When it fires, one of three events occurs:

1. **Mystery Solo Cups** -- 5-12 random positions become mystery symbols that all reveal the same random symbol
2. **High-Pay-Only Respin** -- Remove all low-pay symbols from the reels and respin, guaranteeing only high-pay characters can land
3. **Random Wilds** -- Place 3-8 wild symbols at random positions on the grid

> "Build the Party Foul random event system. It triggers with approximately 1 in 40 probability on any base game spin (check before the spin result is shown). When triggered, randomly select one of three events with equal probability. Event 1 -- Mystery Solo Cups: select 5-12 random grid positions, replace them with mystery symbols, then reveal them all as the same randomly chosen symbol. Event 2 -- High-Pay-Only Respin: remove the spin result, filter the symbol pool to only high-pay symbols (The Pledge through The Frat President), and respin with only those symbols available. Event 3 -- Random Wilds: select 3-8 random grid positions and place wild symbols there, then evaluate wins. Build each event as its own function. Write tests for trigger probability (run 10,000 simulated spins, expect roughly 250 triggers with reasonable variance), mystery symbol reveal consistency, high-pay filtering, and random wild placement."

### Step 8: Animate Each Event

> "Add distinct Framer Motion animations for each Party Foul event. All three start the same way: a 'PARTY FOUL!' banner slams onto the screen with a shake effect. Then: Mystery Solo Cups -- cups appear at the selected positions with a flip animation, then simultaneously reveal the chosen symbol with a burst effect. High-Pay-Only Respin -- low-pay symbols on the grid shatter and fall away, then the reels respin with a premium feel (slower, more dramatic). Random Wilds -- wild symbols rain down onto the grid from above, landing at their positions with a bounce. Each animation should take 2-3 seconds total before win evaluation."

---

## Day 4: xWays Funnels

### Step 9: Build the xWays Engine Logic

xWays Funnels appear on reels 2, 3, and 4 only. A funnel position can expand to show 2-4 copies of the same symbol, effectively adding extra rows to that reel and increasing the ways count.

> "Build the xWays Funnel mechanic. On each spin, reels 2, 3, and 4 have a chance for 1-3 positions to become xWays Funnels (roughly 15% chance per position). Each funnel reveals 2-4 copies of a randomly selected symbol. This expands the effective height of that reel -- if a 4-high reel has one funnel that reveals 3 symbols, that reel effectively has 6 positions. The ways calculation must update dynamically: base is 4x4x4x4x4 = 1024 ways, but xWays can push it higher. Build the funnel placement logic, symbol reveal, dynamic ways calculation, and integrate with win evaluation. Write tests for: ways count calculation (no funnels = 1024, one funnel with 3 reveals on reel 2 = 1536 ways, multiple funnels), correct symbol placement in the expanded grid, and win evaluation with expanded reels."

### Step 10: Animate the Expansion

> "Add animations for xWays Funnels. When a funnel position lands: show a funnel icon that spins and then splits apart, revealing the duplicate symbols. The reel should visually expand -- push other symbols apart to make room, with a smooth Framer Motion layout animation. Display an updated ways counter in the corner of the game (e.g., '1,536 WAYS') that animates when the count changes. The expansion should feel like the grid is stretching to accommodate the extra symbols."

---

## Day 5: Integration and Polish

### Step 11: Make Features Work Together

All four mechanics can occur on the same spin. An xNudge wild can land alongside xWays funnels and a Party Foul trigger. The evaluation order matters:

1. xWays Funnels expand first (changes grid shape)
2. xNudge Wilds nudge into view (on the expanded grid)
3. Party Foul events apply (if triggered)
4. Beer Pong Respin triggers (if two wilds present)
5. Final win evaluation with all multipliers

> "Review the game engine and ensure all four advanced mechanics work together correctly. The evaluation order should be: xWays expansion, xNudge nudging, Party Foul events, Beer Pong Respin check, then win evaluation. Write integration tests that verify: xWays + xNudge (wild nudges on an expanded reel), Party Foul random wilds + xNudge (random wild triggers Beer Pong if it creates two wilds), and a scenario with all features active simultaneously. Make sure multipliers stack correctly and the ways count reflects all expansions."

### Step 12: Animation Sequencing

> "Build an animation queue system that sequences the feature animations correctly when multiple features trigger on the same spin. The order should match the evaluation order: xWays funnels expand, then xNudge wilds nudge, then Party Foul animates, then Beer Pong Respin plays. Each animation should complete before the next begins. Add a 'skip' button that lets the player jump to the final state. Test with a forced scenario where all features trigger at once."

### Step 13: RTP Verification

> "Run a 100,000-spin simulation with all advanced mechanics active. Calculate the RTP and compare it to the target of 96.09%. Log the frequency of each feature triggering and the average win per feature. If the RTP is off by more than 0.5%, identify which mechanic's payout distribution needs adjustment and suggest corrections."

Commit after the RTP is within tolerance.

---

## Acceptance Criteria

By the end of Week 15, the game must have all of these working:

- [ ] **xNudge Wild** -- The Keg Stand nudges into full view with +1 multiplier per nudge
- [ ] **Beer Pong Respin** -- Two wilds lock reels between them, remaining reels respin, multipliers combine
- [ ] **Party Foul events** -- Random trigger (~1/40 spins) with three distinct event types
- [ ] **xWays Funnels** -- Positions on reels 2-4 expand to show 2-4 matching symbols, ways count updates
- [ ] **Framer Motion animations** for every mechanic (nudge, respin, reveal, expand)
- [ ] **Animation sequencing** -- Multiple features on one spin play in correct order with skip option
- [ ] **All features integrate** -- Mechanics can co-occur and multipliers stack correctly
- [ ] **Unit tests** for each mechanic's engine logic
- [ ] **Integration tests** for combined mechanic scenarios
- [ ] **RTP within tolerance** (96.09% +/- 0.5%) verified by simulation
- [ ] **Deployed to Vercel** with all features playable on the live site

---

## Gut Check

Play 50 spins on the live site. Ask yourself:

- Do the features feel distinct from each other? Can you tell which mechanic just fired?
- Does the xNudge multiplier climbing feel exciting?
- Does a Beer Pong Respin feel like a special moment?
- When a Party Foul triggers, does it break the rhythm in a good way?
- Do xWays expansions make the grid feel like it's opening up?

If the mechanics feel like four variations of the same thing, the animations need more personality. If they feel distinct and each one gets a reaction, you're in good shape. Next week you build the bonus rounds that tie it all together.
