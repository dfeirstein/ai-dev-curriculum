# Project: Frat House Frenzy -- Reel UI & v1 Launch

**What you're building:** The visual reel component for Frat House Frenzy -- 5x4 grid rendering, symbol art, spin and cascade animations, win line highlighting, sound design with Howler.js, an admin dashboard for RTP monitoring, and the v1 launch of a playable slot game.

**Time estimate:** 10-12 hours

**What you'll need open:** Ghostty (your terminal) and a web browser.

---

## The Brief

Frat House Frenzy has a complete backend: player accounts, Stripe payments, and a provably fair game engine. But right now, the game is just an API -- numbers in, numbers out. This week you bring it to life. You'll build the visual reel grid, animate spinning and cascading symbols, highlight wins, add sound effects, and wire it all to the game engine API.

You'll also build an admin dashboard for monitoring RTP, player activity, and game health. Then you ship v1 -- a playable slot game with real auth, real payments, provably fair RNG, and real animations.

When you're done, share the URL. This is a real product.

---

## Phase 1: Research

Start Claude Code and research the UI approach:

> "Claude, I need to build a slot machine reel UI in React. The game has a 5x4 grid (5 reels, 4 rows). I need: spinning animation when the player hits spin, symbols landing one reel at a time (left to right), cascade animation where winning symbols disappear and new ones fall in, and win highlighting. What's the best approach? Should I use Framer Motion, CSS animations, or something else?"

Follow up:

> "Claude, how do professional slot games handle the spinning reel illusion? I want the reels to blur/scroll vertically and then decelerate, landing on the final symbols. What animation technique creates this effect in a web app?"

> "Claude, I want to add sound effects to the game using Howler.js. I need sounds for: spin start, reel stop, small win, big win, cascade, and bonus trigger. How do I manage audio sprites and sync sounds with animations?"

> "Claude, how should I structure a game admin dashboard? I need to monitor RTP in real time, track active players, view recent big wins, and see player balance distributions. What charting library works well with Next.js?"

---

## Phase 2: Plan

> "Claude, plan the complete reel UI and v1 launch for Frat House Frenzy. I need:
>
> **Reel Grid Component (5x4):**
> - 5 columns (reels), 4 rows per reel
> - Each cell displays a symbol with its art/icon
> - Dark background with subtle grid lines and neon border
> - Responsive: works on desktop and mobile
>
> **Symbol Rendering:**
> - Each symbol type has a distinct visual (icon or styled emoji for v1, custom art later)
> - Low pays (cups): simple colored circles or cup icons
> - High pays (characters): more detailed icons with character colors
> - Wild (The Keg): golden glow effect
> - Scatter (Noise Complaint): pulsing red/blue police light effect
>
> **Spin Animation:**
> - Player clicks Spin -> reels blur and scroll vertically
> - Reels stop one at a time, left to right, with a slight delay between each
> - Final symbols land with a subtle bounce
> - During spin, the Spin button is disabled and shows a loading state
>
> **Cascade Animation:**
> - Winning symbols flash/highlight, then shrink and disappear
> - Remaining symbols slide down with gravity
> - New symbols drop in from above with a bounce
> - Brief pause between cascades for readability
> - Cascade counter displayed on screen
>
> **Win Presentation:**
> - Winning symbols highlighted with a glow matching their color
> - Win amount animates up (counting up effect) below the grid
> - Big wins (10x+): screen shake, larger text, neon flash
> - Mega wins (50x+): full-screen celebration, particles
>
> **Sound Design (Howler.js):**
> - Spin start: whoosh sound
> - Reel stop: click/thud for each reel
> - Small win: coin clink
> - Medium win: cash register
> - Big win: bass drop + crowd roar
> - Cascade: rising pitch chime for each cascade step
> - Background: lo-fi party ambient (looping, low volume)
> - Mute toggle in the UI
>
> **Game Controls (connect to existing bet system):**
> - Wire the Spin button to POST /api/game/spin
> - Pass the current bet amount from the bet selector (built in Week 9)
> - Display the spin result using the animation pipeline
> - Auto-spin toggle: spin automatically every N seconds
> - Turbo mode: skip animations, show results instantly
>
> **Admin Dashboard:**
> - Protected route at /admin (admin users only)
> - Real-time RTP chart: rolling RTP over last 1,000 / 10,000 / 100,000 spins
> - Active players count and list
> - Recent big wins table (player, amount, multiplier, time)
> - Player balance distribution histogram
> - Total wagered, total paid, house edge for today / this week / all time
> - Use a charting library (Recharts)
>
> Plan the component hierarchy, animation sequencing, sound triggers, API integration, and admin queries."

Review the plan. Key questions:

- What is the animation sequence timeline? (Spin -> stop reels -> check wins -> highlight -> cascade -> repeat)
- How long does each animation phase take? Is the total reasonable (not too slow, not too fast)?
- How are sounds synced to animation events?
- What happens if the player clicks Spin before the previous animation finishes?
- How does the admin dashboard query RTP efficiently on large datasets?

---

## Phase 3: Execute

### Step 1: Symbol Art and Grid Component

> "Claude, build the reel grid component:
> 1. A 5x4 grid using CSS Grid, each cell approximately 80x80px on desktop
> 2. Dark background (#0a0a0a) with subtle neon-blue grid lines
> 3. Neon border around the entire grid (glow effect)
> 4. Each symbol rendered as a styled component:
>    - Red Cup: 🔴 on a red-tinted circle
>    - Blue Cup: 🔵 on a blue-tinted circle
>    - White Cup: ⚪ on a white-tinted circle
>    - Green Cup: 🟢 on a green-tinted circle
>    - The Pledge: 😰 with a yellow border
>    - The DJ: 🎧 with a purple border
>    - Pong King: 🏆 with a gold border
>    - House Mom: 😎 with a pink border
>    - Frat President: 👑 with a neon green border
>    - Wild (The Keg): 🍺 with a golden glow
>    - Scatter (Noise Complaint): 🚨 with a pulsing red/blue glow
> 5. The grid accepts a 5x4 array of symbol IDs and renders them
> 6. Responsive: scales down on mobile
> Make it look polished -- this is the centerpiece of the game."

### Step 2: Spin Animation

> "Claude, add the spin animation to the reel grid:
> 1. When spin is triggered, each reel column starts a vertical scrolling animation (symbols blurring upward)
> 2. Reels start spinning left to right with a 100ms delay between each reel
> 3. Each reel spins for a random duration (0.5-1.5 seconds) before decelerating
> 4. The final symbols (from the API response) are placed at the landing position
> 5. Reels stop left to right with a 200ms delay between each
> 6. Each reel lands with a subtle bounce ease (overshoot then settle)
> 7. Use Framer Motion's animate and transition for smooth physics-based animation
> 8. During spin, show a blur overlay on each reel
> 9. Spin button shows 'Spinning...' and is disabled until all reels stop"

Test:
- Click spin -- do the reels animate smoothly?
- Do reels stop one at a time, left to right?
- Is there a satisfying bounce on landing?
- Does the spin button re-enable after all reels stop?

### Step 3: Cascade Animation

> "Claude, add the cascade animation:
> 1. After the initial spin lands and wins are detected, highlight winning symbols with a glow (1 second)
> 2. Winning symbols shrink to 0 scale and fade out (0.3 seconds)
> 3. Remaining symbols in each reel slide down to fill the gaps (0.3 seconds, with gravity easing)
> 4. New symbols appear at the top of each reel and drop into place (0.3 seconds, with bounce)
> 5. Check for new wins -- if found, repeat from step 1
> 6. Display a cascade counter ('Cascade 1', 'Cascade 2', etc.) that increments with each step
> 7. Pause 0.5 seconds between cascades for readability
> 8. The cascade data comes from the API response -- each step's grid and wins are pre-calculated
> Use Framer Motion's AnimatePresence for enter/exit animations."

Test:
- Trigger a spin that produces a cascade (may need to use a test endpoint or mock data)
- Do winning symbols highlight, then disappear?
- Do remaining symbols fall down smoothly?
- Do new symbols drop in from above?
- Does the cascade counter update?

### Step 4: Win Presentation

> "Claude, build the win presentation system:
> 1. After each spin/cascade resolves, show the total win amount below the grid
> 2. Win amount animates with a counting-up effect (0.00 -> final amount over 1 second)
> 3. Color coding: small wins in white, medium wins (5x+) in neon green, big wins (10x+) in neon pink, mega wins (50x+) in gold
> 4. For big wins (10x+ bet): add screen shake (subtle CSS transform), larger text, flash the grid border
> 5. For mega wins (50x+ bet): full-screen overlay with particles (confetti), celebration text, dramatic pause before showing the amount
> 6. Win amounts persist on screen for 3 seconds after the final cascade, then fade
> 7. Winning symbols on the final grid keep a subtle pulsing glow until the next spin"

### Step 5: Sound Design with Howler.js

> "Claude, integrate Howler.js for sound effects:
> 1. Install howler and create a sound manager singleton
> 2. Load these sounds (use placeholder royalty-free sounds for now):
>    - spin-start: short whoosh
>    - reel-stop: mechanical click (play once per reel stopping)
>    - win-small: coin clink
>    - win-medium: cash register ding
>    - win-big: bass drop with crowd
>    - cascade: rising pitch chime (pitch increases with cascade step)
>    - bonus-trigger: record scratch
>    - ambient: lo-fi party background (loop)
> 3. Sync sounds to animation events:
>    - Spin button clicked -> play spin-start
>    - Each reel stops -> play reel-stop
>    - Win detected -> play appropriate win sound based on size
>    - Cascade starts -> play cascade sound with increasing pitch
> 4. Mute/unmute toggle in the game UI (persist preference in localStorage)
> 5. Volume control slider
> 6. Sounds should not overlap awkwardly -- manage concurrent audio"

Test:
- Spin with sound on -- do sounds sync with the animation?
- Toggle mute -- does it silence everything?
- Does the ambient loop play continuously?
- Do cascade sounds increase in pitch?

### Step 6: Connect to Game Engine API

> "Claude, wire the reel UI to the game engine API:
> 1. Spin button calls POST /api/game/spin with the current bet amount
> 2. On response, feed the initial grid to the spin animation
> 3. Feed each cascade step to the cascade animation sequentially
> 4. Update the balance display after the spin completes
> 5. Update session stats (total bets, total wins, spin count)
> 6. Handle errors: if the spin API fails, show an error toast and re-enable the spin button
> 7. Handle insufficient balance: show a 'Deposit' prompt instead of spinning
> 8. Display the provably fair info: server seed hash before spin, and a 'Verify' link after
> 9. Auto-spin mode: after one spin completes, automatically trigger the next (with a toggle to enable/disable)
> 10. Turbo mode: skip all animations, just show the final grid and total win instantly"

Test the complete flow:
- Deposit some test funds
- Hit Spin -- does the full animation play with the real API result?
- Does the balance update correctly?
- Does the provably fair hash show before the spin?
- Try auto-spin -- does it keep going?
- Try turbo mode -- does it skip animations?

### Step 7: Admin Dashboard

> "Claude, build the admin dashboard at /admin:
> 1. Restrict access to admin users -- redirect non-admins to the game lobby
> 2. Summary cards at the top: total players, active sessions now, total wagered today, house edge today
> 3. RTP chart (Recharts line chart): rolling RTP calculated over the last 1,000 spins, updated every 30 seconds. Show the target line at 96.09%
> 4. Recent big wins table: last 20 wins over 10x bet, showing player (anonymized), bet amount, win amount, multiplier, timestamp
> 5. Player balance distribution: histogram showing how many players are in each balance range ($0-10, $10-50, $50-200, $200+)
> 6. Revenue summary: total deposits, total withdrawals, net revenue -- today, this week, all time
> 7. Active sessions list: player ID, session duration, spins this session, current balance
> Style with the dark theme to match the rest of the app. Use neon accent colors for chart lines."

Test:
- Can an admin see the dashboard?
- Does a non-admin get redirected?
- Do the charts display real data?
- Does the RTP chart update periodically?

### Step 8: Provably Fair Verification Page

> "Claude, build a provably fair verification page at /verify:
> 1. Explain the provably fair system in plain language: what it means, how it works, why the player can trust it
> 2. Show the player's current client seed with an option to change it
> 3. Show the current server seed hash (commitment)
> 4. Verification tool: input fields for server seed, client seed, and nonce
> 5. Calculate button that reproduces the grid and shows the result
> 6. Compare against the actual spin result from the database
> 7. Link to the player's spin history with 'Verify' buttons on each past spin
> This is the trust layer. It must be clear enough for a non-technical player to understand."

### Step 9: Quality Gates

> "Claude, run npm run lint and fix any issues."

> "Claude, run npm run typecheck and fix any issues."

> "Claude, run npm run build and make sure it completes without errors."

### Step 10: Ship v1

> "Claude, commit all changes with a clear message about the reel UI and v1 launch, push to GitHub, and deploy to Vercel."

After deployment, run through the complete player journey on the live URL:

1. Sign up as a new player
2. View the game lobby -- dark theme, neon accents, balance display
3. Deposit test funds via Stripe ($50)
4. Enter the game -- see the 5x4 grid
5. Set a bet amount ($1.00)
6. Hit Spin -- watch the reel animation
7. If you win, watch the cascade animation and win presentation
8. Check that sounds play and sync with the animation
9. Spin several more times -- does the balance update correctly?
10. Check the provably fair page -- does verification work?
11. Try a bonus buy (if balance allows)
12. Check the wallet -- is every transaction recorded?
13. Visit the admin dashboard -- do the charts show real data?
14. Check the session history -- are all spins recorded?
15. Log out and log back in -- is everything persistent?

If everything works, v1 is shipped.

---

## Acceptance Criteria

Your project is complete when all of these are true:

- [ ] 5x4 reel grid renders all symbol types with distinct visuals
- [ ] Spin animation plays with reels stopping left to right
- [ ] Cascade animation removes winning symbols, drops remaining, fills from top
- [ ] Win amounts display with counting-up animation and size-based styling
- [ ] Big wins trigger screen effects (shake, flash, particles for mega wins)
- [ ] Howler.js sound effects play in sync with animations
- [ ] Mute toggle and volume control work and persist
- [ ] Spin button connects to the real game engine API
- [ ] Balance updates correctly after each spin
- [ ] Auto-spin and turbo mode work
- [ ] Provably fair verification page explains the system and allows verification
- [ ] Admin dashboard shows RTP chart, big wins, player stats, and revenue
- [ ] Admin dashboard restricted to admin users
- [ ] Full player journey works end-to-end: signup -> deposit -> play -> verify -> withdraw
- [ ] `npm run lint` passes
- [ ] `npm run typecheck` passes
- [ ] `npm run build` completes without errors
- [ ] App is deployed and functional on Vercel
- [ ] Code is on GitHub

---

## Stretch Goals

**Add Custom Symbol Art**
> "Claude, replace the emoji symbols with custom SVG icons or pixel art. Create a consistent art style: gritty cartoon with neon outlines. Each symbol should be immediately recognizable at the small grid size. Use SVG for crisp rendering at any scale."

**Add a Win Replay Feature**
> "Claude, add the ability to replay any past spin's animation. From the spin history, clicking 'Replay' re-runs the full animation sequence (spin, cascades, win presentation) with the historical data. This is great for sharing big wins."

**Add Particle Effects**
> "Claude, add particle effects using tsparticles or a canvas-based system. Gold coins burst from winning symbols. Confetti on big wins. Subtle ambient particles (like dust motes in party lighting) float across the screen during play."

**Add Mobile-Optimized Controls**
> "Claude, optimize the game for mobile play. Stack the bet controls below the grid, make the spin button large and thumb-friendly, add swipe gestures (swipe up to spin), and ensure all animations run smoothly at 60fps on mobile devices."

---

## Troubleshooting

**Animations janky or dropping frames**
> "Claude, the reel animations are stuttering. Check: are we animating expensive CSS properties (width, height) instead of transforms? Switch to transform: translateY() for reel scrolling. Use will-change: transform on animated elements. Reduce the number of simultaneously animated elements."

**Sounds not playing**
> "Claude, Howler.js sounds don't play. Check: are the sound files loaded before the first spin? Browsers require a user interaction before playing audio -- make sure the first sound plays in response to the Spin button click event, not on page load. Check the browser console for audio context errors."

**Spin button clickable during animation**
> "Claude, I can click Spin while the previous spin is still animating, causing overlapping animations. Add a 'spinning' state that disables the button from the moment it's clicked until the entire animation sequence (including all cascades) completes. Check that auto-spin also respects this state."

**Admin dashboard queries too slow**
> "Claude, the admin dashboard takes forever to load. The RTP calculation is scanning every spin in the database. Add an index on spins.createdAt and use a materialized view or cache for the rolling RTP calculation. For the histogram, use database aggregation instead of fetching all balances to the client."

**Cascade animation out of sync with data**
> "Claude, the cascade animation shows the wrong symbols after a cascade step. The animation is using stale grid data. Make sure each cascade step reads from the correct index in the cascade chain array returned by the API. Log the grid state at each step to compare with what's rendered."

---

## Reflection

This is v1. A complete, playable, provably fair slot game:

- Player authentication with Better Auth
- Stripe deposits and withdrawals
- Provably fair RNG with player-verifiable outcomes
- 5x4 reel grid with spin and cascade animations
- Ways-to-win calculation with correct payout math
- Sound design synced to game events
- Real-time balance management with full audit trail
- Admin dashboard with RTP monitoring
- Deployed and live on Vercel

You directed the construction of all of this. You didn't write the game engine math by hand. You didn't hand-code the animation timelines. You understood the concepts -- provably fair cryptography, ways-to-win calculation, cascading mechanics, payment processing -- described what you wanted, verified the output, and shipped it.

Step back and look at the stack you just built: Next.js, TypeScript, Tailwind, Better Auth, PostgreSQL, Drizzle, Stripe, Howler.js, Framer Motion, Recharts, Sentry, PostHog. These are production tools used by real companies. The game you built uses the same architecture as real online slot platforms.

Take a moment. Share the URL. This is real.
