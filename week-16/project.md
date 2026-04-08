# Project: Frat House Frenzy — Bonus Rounds & Polish (Week 2/2)

**What you're building:** The complete bonus system, sound design, and visual polish that turn Frat House Frenzy into a finished game.

**Time estimate:** 15-20 hours across the week

**What you'll need open:** Ghostty, your browser, Chrome DevTools, the game design brief (`reference/game-design-brief.md`)

---

## The Brief

The base game mechanics are done. This week you build everything that makes players chase the bonus -- the free spin modes, the escalation system, the Blackout Meter, the Tip Jar Collector, and the Full Send Jackpot. Then you layer on the sound design and visual polish that make the whole experience feel like a real product.

This is the week where the game stops feeling like a tech demo and starts feeling like something someone would actually play.

---

## Phase 1: Bonus Mode Entry (Day 1)

### Step 1: Build the Scatter/Trigger System

Three or more Noise Complaint scatter symbols trigger a bonus round. The number of scatters determines which mode:

- 3 scatters = Party Mode (8 free spins, 5x4 grid)
- 4 scatters = Darty Mode (12 free spins, 5x5 grid)
- 5 scatters = Full Send Mode (12 free spins, 5x6 grid, 3x starting multiplier)

> "Build the bonus round trigger system. The Noise Complaint scatter symbol triggers bonus rounds when 3 or more land on a single spin. 3 scatters = Party Mode (8 free spins, base 5x4 grid). 4 scatters = Darty Mode (12 free spins, starts at 5x5 grid). 5 scatters = Full Send Mode (12 free spins, starts at 5x6 grid, 3x global multiplier from the start). Build the state machine for bonus mode: track mode type, remaining spins, current grid size, and global multiplier. Scatters should be evaluated before cascades remove them. Write tests for each trigger condition and verify that scatter counts of 0, 1, and 2 do not trigger a bonus."

### Step 2: Build the Bonus Buy UI

> "Add a Bonus Buy panel to the game UI. Four purchase options: Party Mode for 80x bet, Darty Mode for 200x bet, Full Send Mode for 500x bet, and 'YOLO' for 69x bet (triggers a random mode with equal probability). Each option shows a button with the mode name, price, and a brief description. Deduct the cost from the player's balance and immediately enter the selected bonus mode. Add a confirmation dialog before purchase. Disable Bonus Buy if the player's balance is below the cheapest option. Style it to match the game's neon party aesthetic."

### Step 3: Animate the Bonus Entry

> "Create a bonus entry animation sequence. When 3+ scatters land: freeze the reels, spotlight the scatter symbols one by one with a police siren flash effect, then transition to a full-screen mode announcement. Party Mode: 'PARTY MODE' with a house party backdrop. Darty Mode: 'DARTY MODE' with an outdoor daylight party scene. Full Send Mode: 'FULL SEND' with maximum chaos -- strobes, screen shake, the works. Each announcement holds for 2 seconds, then the grid transitions to the bonus grid size (animate rows being added from the bottom if the grid expands). Add a skip option."

---

## Phase 2: Bonus Mechanics (Day 2-3)

### Step 4: Party Escalation System

During any bonus mode, landing 3+ scatters again (retrigger) adds a row to the grid and +2 to the global multiplier. Maximum grid height is 7 rows (5x7 = 16,807 ways).

> "Build the Party Escalation system for bonus rounds. When 3+ scatters land during a bonus round: add the scatter count as extra free spins, add one row to the grid (up to max height of 7), and add +2 to the global multiplier. The global multiplier applies to ALL wins for the rest of the bonus. The grid expansion should update the ways calculation dynamically. Write tests for: retrigger adds spins correctly, grid expands from 5x4 to 5x5 to 5x6 to 5x7 and stops, global multiplier accumulates across multiple retriggers, and ways count updates with each expansion."

### Step 5: Blackout Meter

> "Build the Blackout Meter. It's a visual meter (0-100%) that fills during bonus rounds. Each cascade (tumble win) adds 10% to the meter. When it reaches 100%: trigger a Blackout Round -- 3 spins where all wilds that land become sticky (stay in place for all 3 spins). After the 3 Blackout spins, the meter resets to 0%. The meter should persist across the entire bonus round, not reset between spins. Write tests for: meter increments correctly per cascade, meter triggers at exactly 100%, sticky wilds persist for all 3 Blackout spins, meter resets after Blackout completes, and meter carries over between regular bonus spins."

### Step 6: Animate the Blackout

> "Add animations for the Blackout Meter and Blackout Round. The meter should be a vertical bar on the side of the game grid with a neon fill that pulses as it climbs. At 75% it starts glowing red. At 100%: the screen goes dark (actual blackout effect), then slowly illuminates with UV/blacklight coloring -- all symbols get a neon glow-on-dark treatment. Sticky wilds during the Blackout should have a pulsing anchor effect. When the Blackout ends, transition back to normal lighting with a slow fade."

### Step 7: Tip Jar Collector

> "Build the Tip Jar Collector mechanic. The Tip Jar is a special symbol that can land during bonus rounds. When it lands: it absorbs all visible multiplier values on the grid (from xNudge wilds, global multiplier instances, etc.) and becomes a wild symbol with the combined multiplier value. For example: if xNudge wilds show 3x and 2x on the grid, the Tip Jar absorbs both and becomes a wild with 5x multiplier. It stays in position for the current cascade sequence. Write tests for: Tip Jar collects multipliers from multiple sources, becomes a wild after collection, combined multiplier applies to wins through its position, and it correctly identifies all multiplier sources on the grid."

### Step 8: Full Send Jackpot Path

> "Build the Full Send Jackpot trigger. It activates when two conditions are met simultaneously during a bonus round: the global multiplier reaches 15x or higher AND a Tip Jar collects 100x or more in a single spin. When triggered: lock the grid at 5x7 (expand if not already there), give the player 5 guaranteed spins. During Full Send Jackpot: all wilds are sticky, all multipliers are cumulative (they add together across spins rather than resetting). This is the path to the 42,069x maximum win. Write tests for: trigger conditions (both must be met, either alone doesn't trigger), grid locks at 5x7, sticky wild persistence across all 5 spins, cumulative multiplier behavior, and a theoretical max-win calculation."

---

## Phase 3: Sound Design (Day 4)

### Step 9: Set Up Howler.js

> "Set up Howler.js for the game's sound system. Create a SoundManager class that handles: loading audio sprites, playing/stopping sounds, volume control, and mute toggle. The manager should preload all game sounds on first user interaction (to handle browser autoplay restrictions). Add a sound settings panel with master volume, music volume, and SFX volume sliders, plus a mute button. Persist settings to localStorage."

### Step 10: Base Game Sounds

> "Add sound effects for the base game. Use placeholder sounds for now (we'll describe what each should sound like for sourcing later). Sounds needed: reel spin start (whoosh), reel stop (thunk, one per reel with slight delay), small win (coin clink), medium win (cash register), big win 10x+ (crowd cheer), cascade/tumble (symbols falling, glass breaking), xNudge (sliding metal sound per nudge), Beer Pong Respin (reels locking = click, respin = faster whoosh), Party Foul trigger (record scratch), xWays expansion (stretching rubber sound). Use Howler.js audio sprites for efficient loading. Each sound should have a defined volume relative to others."

### Step 11: Bonus and Atmosphere Sounds

> "Add the atmospheric sound layer. Base game background: lo-fi muffled party music, low and ambient. During cascades: EDM build-up that rises in pitch and tempo with each consecutive cascade in a chain. Big win (50x+): bass drop with crowd roar and a brief strobe sound effect. Bonus entry: record scratch to silence, then a 'LET'S GO' shout, then a beat drop that transitions into bonus music. Bonus round background: higher energy EDM loop. Blackout Round: switch to a heartbeat sound with a low drone that builds in intensity over the 3 spins. Full Send Jackpot: maximum energy -- the most intense music loop you have. Build a music state machine that crossfades between these layers based on game state."

---

## Phase 4: Visual Polish (Day 5)

### Step 12: Background Transitions

> "Add dynamic background transitions. Base game: dark frat house interior with dim party lighting and subtle animated elements (flickering neon signs, slow-moving fog). Bonus round: the background should shift to a more chaotic party scene -- brighter lights, more movement. Darty Mode: shift to an outdoor daylight scene with harsh sunlight. Full Send Mode: maximum visual intensity. Blackout Round: near-total darkness with UV glow. Use CSS transitions and Framer Motion for smooth crossfades between states. The background should never distract from the reels but should reinforce the current game state."

### Step 13: Cascade Chaos Effects

> "Add visual effects that escalate with consecutive cascades. First cascade: subtle sparkle on winning symbols. Second cascade: winning symbols explode with more particle effects. Third cascade: screen border starts glowing. Fourth+ cascade: screen shake increases, particle density climbs, the entire UI feels like it's barely holding together. Reset the escalation when the cascade chain ends. These effects should complement the EDM build-up from the sound system. Use CSS animations and Framer Motion -- keep it performant."

### Step 14: Win Presentation

> "Build a tiered win presentation system. Small win (under 5x): brief coin animation, win amount flashes in place. Medium win (5x-20x): win amount flies to the balance counter, coin shower effect. Big win (20x-50x): full-screen overlay with the win amount counting up, confetti particles. Mega win (50x-100x): more dramatic version with bass drop, screen flash, extended counting animation. Epic win (100x+): the most intense celebration -- screen goes white, dramatic reveal of the amount, sustained particle effects, the balance counter spins. Full Send (1000x+): unique one-time animation with maximum spectacle. Each tier should take progressively longer to present. Add a 'click to skip' on all of them."

### Step 15: Final Integration Pass

> "Do a final integration pass on the entire game. Verify: all sounds trigger at the correct moments and don't overlap inappropriately, background transitions are smooth, cascade effects reset properly, win presentations match the actual win tier, bonus mode entry/exit transitions are clean, the Blackout visual effect works with all other effects active, and performance stays smooth (target 60fps) even during the most complex scenarios. Run Lighthouse on the game page and fix any performance issues. Test on mobile -- the game should be playable on a phone screen."

---

## Acceptance Criteria

By the end of Week 16, the complete game must have:

- [ ] **Three bonus modes** -- Party Mode, Darty Mode, Full Send Mode with correct scatter triggers
- [ ] **Bonus Buy UI** -- Four purchase options with confirmation dialog
- [ ] **Party Escalation** -- Retriggers add rows (up to 5x7) and +2 global multiplier
- [ ] **Blackout Meter** -- Fills with cascades, triggers 3-spin sticky-wild round at 100%
- [ ] **Tip Jar Collector** -- Absorbs visible multipliers, becomes wild with combined value
- [ ] **Full Send Jackpot** -- Triggers at 15x global + 100x Tip Jar, 5 guaranteed spins with cumulative multipliers
- [ ] **Sound design** with Howler.js -- base game SFX, atmospheric layers, EDM builds, Blackout heartbeat
- [ ] **Music state machine** -- Crossfades between game states (base, bonus, Blackout, Full Send)
- [ ] **Background transitions** -- Visual environment shifts per game state
- [ ] **Cascade chaos effects** -- Escalating visual intensity per cascade chain
- [ ] **Tiered win presentations** -- Six tiers from small to Full Send
- [ ] **Unit tests** for all bonus mechanics
- [ ] **RTP verification** with bonus rounds active (96.09% +/- 0.5%)
- [ ] **60fps performance** during the most complex game states
- [ ] **Deployed to Vercel** with the complete game playable

---

## Reflection

You've built a complete, feature-rich game with professional-grade sound and visual design. Look at what's in this project:

- A provably fair game engine with extreme-volatility math
- Five distinct base game mechanics that interact with each other
- Three bonus modes with escalation, meters, collectors, and a jackpot path
- A layered sound system that responds to game state
- Visual effects that escalate with gameplay intensity
- Tiered win presentations that match the emotional stakes

That's not a tutorial exercise. That's a product. You built it by describing what you wanted to an AI and verifying the results. The same workflow that built a SaaS app built a game engine. The pattern is the same -- the domain doesn't matter.
