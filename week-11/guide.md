# Week 11 Guide: Frat House Frenzy — Reel UI & v1 Launch

Last week you built the brain — the game engine that transforms a bet into a provably fair outcome. This week you build the body: the visual experience that makes spinning feel like a party. Reels that spin and stop left to right, symbols that cascade and explode, sounds that thump and build, and a dashboard that watches the math in real time.

By the end of this week, Frat House Frenzy is playable. Not "demo playable" — actually playable. Authenticated, funded, provably fair, and animated. That's v1.

---

## Part 1: Building the Reel Grid with React

### The Grid Component

The game UI centers on a 5×4 grid of symbols. Each cell shows a symbol — a cup, a character, a wild, a scatter. The grid component is the most important visual element in the entire game.

At rest, the grid is static: 20 symbols arranged in 5 columns and 4 rows. During a spin, the reels animate. After the spin resolves, winning symbols highlight and cascade.

### Thinking in Components

Break the UI into a component hierarchy:

- **GameScreen** — The full game view. Contains the reel grid, bet controls, balance display, and win display.
- **ReelGrid** — The 5×4 grid container. Manages the layout of all reels.
- **Reel** — A single column of 4 symbols. Each reel handles its own spin animation.
- **SymbolCell** — A single symbol position. Displays the symbol image/icon, handles highlight states (normal, winning, removing).
- **BetControls** — Bet amount selector, spin button, auto-spin toggle, turbo mode toggle.
- **BalanceDisplay** — Shows current balance and last win amount.
- **WinDisplay** — Shows win announcements ("BIG WIN — 50x!") with animations.

### Symbol Rendering

Each symbol needs a visual representation. For v1, this can be illustrated icons or styled text/emoji placeholders — the art can be refined later. What matters is that each symbol is visually distinct and recognizable at a glance.

The solo cups (low pays) should look similar to each other but differ by color. The characters (high pays) should be immediately distinguishable — different silhouettes, different color palettes. Wilds and scatters need to stand out from everything else.

### When Directing Claude Code

> "Claude, build the reel grid UI for Frat House Frenzy. Create a GameScreen component containing a 5×4 ReelGrid, BetControls (bet selector, spin button), and BalanceDisplay. Each symbol type should have a distinct visual representation. Use Tailwind for layout and styling. The grid should have a dark theme with neon party lighting accents — think gritty cartoon meets nightclub."

---

## Part 2: Animation with Framer Motion

### Why Animation Matters in a Slot Game

In most web applications, animation is a nice-to-have. In a slot game, animation is the product. The experience of watching reels spin, stop, and reveal your fate is the entire reason someone plays. Without animation, you just have a grid of icons that changes — that's a spreadsheet, not a game.

### Framer Motion Basics

Framer Motion is a React animation library. You replace standard HTML elements with motion elements (`motion.div`, `motion.span`) and describe how they should animate. You define states, and Framer Motion handles the transitions between them.

You don't need to understand every Framer Motion API. You need to understand what animations the game needs, and Claude Code handles the implementation.

### The Spin Animation

When the player clicks spin, the reels need to animate in sequence:

1. **All reels start spinning** — symbols blur and scroll downward rapidly, like a real slot machine.
2. **Reels stop left to right** — Reel 1 stops first, then reel 2, then 3, 4, 5. Each stop has a brief "bounce" or "slam" effect. The staggered stop creates anticipation — you see the first symbols land and start hoping the next reels complete a match.
3. **Landing effect** — When a reel stops, there's a subtle bounce or thud animation. The symbols settle into place.

The outcome was determined by the server before the animation starts. The animation is purely cosmetic — it reveals a result that already exists.

### The Cascade Animation

After wins are identified:

1. **Winning symbols highlight** — A glow, pulse, or flash effect draws attention to the winning positions.
2. **Winning symbols are removed** — They explode, dissolve, shatter, or fly off the grid. This should feel satisfying.
3. **Remaining symbols drop** — Gravity pulls them down to fill the gaps. This should feel physical — a slight acceleration and bounce at the bottom.
4. **New symbols arrive** — They drop in from above to fill the empty positions at the top.
5. **Evaluate again** — If new wins form, repeat the cascade animation.

Each cascade step should be clearly visible. The player needs to see what won, see it removed, see new symbols arrive, and see if they won again. Speed this up, but don't skip it.

### Win Celebrations

Different win sizes deserve different celebrations:

- **Small win (under 5x):** Brief highlight and payout counter.
- **Medium win (5x–20x):** Bigger highlight, the win amount animates up in a larger font.
- **Big win (20x–50x):** Full-screen overlay, "BIG WIN" text with particle effects, a dramatic payout counter that ticks up.
- **Mega win (50x+):** Extended celebration — screen shake, bass drop, strobing effects, crowd sounds, the payout counter ticks up slowly for maximum drama.

### When Directing Claude Code

> "Claude, add Framer Motion animations to the reel grid. Implement: (1) spin animation where reels scroll and stop left-to-right with a bounce, (2) cascade animation where winning symbols highlight then explode, remaining symbols drop with gravity, and new symbols enter from above, (3) win celebration overlays that scale with win size — small text for small wins, full-screen celebration for big wins. Use Framer Motion's AnimatePresence for symbol enter/exit."

---

## Part 3: Sound Design with Howler.js

### Why Sound Matters

Close your eyes and think of a slot machine. You probably hear it — the spinning, the landing, the coin sounds, the victory jingles. Sound is half the experience. A silent slot game feels broken, no matter how good the visuals are.

Sound creates emotional feedback loops. The satisfying "thud" when reels land tells you something happened. The building music during cascades creates tension. The bass drop on a big win delivers a dopamine hit. Game designers have understood this for decades.

### Howler.js

Howler.js is a JavaScript audio library that handles the messy reality of browser audio. Different browsers support different formats, mobile has autoplay restrictions, and playing multiple sounds simultaneously requires careful management. Howler.js abstracts all of this.

### Audio Sprites

Instead of loading 50 individual sound files, audio sprites combine many sounds into one file. Each sound is a segment within that file, defined by start time and duration. This reduces HTTP requests and load time.

Think of it like a vinyl record with track markers — one file, many sounds, instant access to any of them.

### Sound States for Frat House Frenzy

The game has distinct audio moods:

**Base game.** Lo-fi party background: muffled bass, distant chatter, ambient college-party vibes. This plays continuously during normal spins. It should be unobtrusive — a mood setter, not a distraction.

**Spin sounds.** Reel spin (a whirring/clicking sound), reel stop (a satisfying thud for each reel), symbol land (subtle click).

**Cascade sounds.** EDM build-up that increases in pitch and tempo with each consecutive cascade. First cascade: subtle. Third cascade: building. Fifth cascade: the music is clearly escalating. This creates the feeling of chaos building.

**Win sounds.** Scaled to the win size:
- Small win: brief coin/ding sound.
- Big win (50x+): bass drop, crowd roar, the music shifts to celebration mode.
- The payout counter should have its own ticking sound that speeds up as the number climbs.

**Bonus entry.** Record scratch → beat of silence → "LET'S GO" voice sample → beat drop. This is the most dramatic audio moment in the game.

**Blackout round.** Heartbeat effect with a low drone that builds in intensity.

### When Directing Claude Code

> "Claude, integrate Howler.js for sound in Frat House Frenzy. Set up an audio sprite system. Implement sound states: base game ambient loop, spin/stop/land effects, cascade sounds that build in intensity with each cascade level, win celebration sounds scaled to win size, and a bonus entry sound sequence. Add a volume control and mute toggle. Make sure sounds respect browser autoplay policies by starting audio context on first user interaction."

---

## Part 4: State Management for a Game

### Why Game State Is Different

A typical web app has straightforward state: form values, user data, UI toggles. A slot game has layered state that changes rapidly and must stay synchronized between the server and the client.

### The State Layers

**Player state** — persistent, stored in the database:
- Balance
- Current bet amount
- Client seed
- Session history

**Spin state** — temporary, lives during a single spin:
- Is the game currently spinning?
- Current cascade level
- Accumulated wins for this spin
- The grid state at each cascade step

**Animation state** — drives the visual layer:
- Which reels are spinning vs. stopped?
- Which symbols are highlighted as winners?
- Which symbols are being removed (cascade out)?
- Which symbols are dropping in (cascade in)?
- Is a win celebration playing?
- Current celebration tier (small/big/mega)

**UI state** — user preferences:
- Auto-spin on/off (and remaining count)
- Turbo mode on/off
- Sound on/off and volume level
- Bet amount selection

### The Flow

1. Player clicks spin → spin state becomes "spinning," animation state starts reel animations.
2. Server returns result → spin state gets the outcome, animation state begins stopping reels.
3. Reels stop → animation state highlights wins, spin state tracks cascade level.
4. Cascade animations play → animation state cycles through remove/drop/new for each cascade.
5. All cascades complete → spin state calculates final payout, player state updates balance.
6. If auto-spin is on, loop back to step 1.

### When Directing Claude Code

> "Claude, set up state management for Frat House Frenzy. Use React state (or Zustand if complexity warrants it) to manage: player state (balance, bet amount), spin state (spinning/idle, cascade level, accumulated wins), animation state (reel states, symbol highlights, celebration tier), and UI state (auto-spin, turbo mode, sound settings). The spin state machine should have clear transitions: idle → spinning → revealing → cascading → celebrating → idle."

---

## Part 5: The Admin Dashboard

### What the Operator Needs to See

You built the player-facing game. Now build the operator-facing dashboard — the control room where you monitor whether the game is behaving correctly and whether the business is healthy.

### RTP Monitoring in Real-Time

The most important metric for a game operator is actual RTP. Your math model targets 96.09%, but does the live game match?

Build a chart (using Recharts) that shows:
- **Rolling RTP** — the actual return to player over the last 1,000 / 10,000 / 100,000 spins. Short windows will fluctuate wildly (that's normal with extreme volatility). Longer windows should converge toward 96.09%.
- **Cumulative RTP over time** — a line chart showing how RTP has evolved since launch. It should trend toward the target.
- **RTP by time period** — hourly, daily, weekly breakdowns.

If actual RTP deviates significantly from the target over a large sample, something is wrong with the math model.

### Player Activity Metrics

- **Active players** — how many players are currently in a session.
- **Spins per hour** — total game throughput.
- **Average bet size** — what players are actually wagering.
- **Player sessions** — average session length, session count.

### Revenue Metrics

- **Total wagered** (handle) — all bets placed.
- **Total paid out** — all winnings returned.
- **Gross gaming revenue** (GGR) — handle minus payouts. This is your revenue.
- **GGR by day/week/month** — the trend line.

### Building Charts with Recharts

Recharts is a React charting library built on D3. It provides line charts, bar charts, area charts, and more as React components. You describe the data and the chart type, and Recharts renders it.

For the admin dashboard:
- Line chart for RTP over time
- Bar chart for daily GGR
- Area chart for spin volume by hour
- Number cards for key metrics (active players, current RTP, today's GGR)

### When Directing Claude Code

> "Claude, build an admin dashboard at /admin for Frat House Frenzy. Use Recharts for visualizations. Include: (1) a rolling RTP chart showing actual RTP over the last 1K/10K/100K spins with a target line at 96.09%, (2) a GGR chart showing daily gross gaming revenue, (3) player activity metrics (active players, spins per hour, average bet), (4) number cards for key metrics. Protect the admin route with role-based access."

---

## Part 6: The Provably Fair Verification Page

### Letting Players Verify Their Spins

Trust isn't just about having a provably fair system — it's about making it accessible. Players should be able to verify any spin they've made without being cryptography experts.

### The Verification Page

Build a page where players can:

1. **See their spin history** — a table showing each spin's date, bet, result, and payout.
2. **View the provably fair details** for any spin — server seed hash (shown before the spin), revealed server seed (shown after the session), client seed, and nonce.
3. **Run the verification** — click a button and the page recalculates the outcome using the same HMAC-SHA256 formula. It shows the raw hex output, the symbol mapping, and the resulting grid — matching what the player saw.
4. **Change their client seed** — for future spins, the player can enter their own client seed. This guarantees the server can't have pre-computed outcomes for their specific seed.

### Making It Understandable

The verification page should explain the process in plain language:

- "Before your spin, the server committed to a secret seed. Here's the hash it showed you."
- "After your session, the server revealed the actual seed. You can verify the hash matches."
- "Using the server seed, your client seed, and the spin number, the math produces this exact outcome."
- "If anything doesn't match, the game was tampered with. (It won't be.)"

Include a "Verify" button that runs the calculation client-side so the player can confirm independently.

### When Directing Claude Code

> "Claude, build a provably fair verification page at /verify. Show the player's spin history with details for each spin. For each spin, display the server seed hash, revealed server seed, client seed, and nonce. Add a 'Verify' button that recalculates the HMAC-SHA256 output client-side and shows the resulting symbol grid. Include a section where players can change their client seed for future sessions. Add plain-language explanations of the provably fair process."

---

## Part 7: What "v1 Launch" Means for a Game

### The v1 Checklist

v1 of Frat House Frenzy is not "finished." It's "complete enough to be real." Here's what v1 must have:

**Playable:**
- Players can spin with real bets
- The 5×4 grid renders correctly
- Ways-to-win evaluation works
- Cascades work
- Payouts are mathematically correct

**Authenticated:**
- Players can sign up and log in
- Each player has their own balance and history
- Sessions are secure

**Funded:**
- Players can deposit via Stripe
- Balance updates correctly on wins and losses
- Withdrawals work (or are queued for manual processing)

**Provably fair:**
- Server seed commitment before each session
- Full verification available to players
- Seeds stored and revealed correctly

**Animated:**
- Reels spin and stop with animation
- Cascades are visually clear
- Win celebrations exist (even basic ones)
- Sound plays for key events

**Monitored:**
- Admin dashboard shows RTP and key metrics
- Sentry catches errors
- Spin logs exist for audit

### What v1 Does NOT Need

- Every bonus feature (Beer Pong Respin, Party Foul, xWays Funnels — these come in later weeks)
- Perfect art (placeholder symbols are fine)
- Mobile optimization
- The Full Send Jackpot
- Bonus rounds (Party Mode, Darty Mode, Full Send Mode)
- The Blackout Meter

These are powerful features that will make the game special. But they're enhancements to a working game. You need the working game first.

### The Trap of "It's Not Ready Yet"

The most common reason games never launch is scope creep disguised as quality. "We need one more feature" becomes ten more features. v1 exists so you can play it, show it to people, get feedback, and discover what actually matters. Half the features you think are essential will turn out to be irrelevant. Half the features you haven't thought of will turn out to be critical. You can only discover this by launching.

---

## Part 8: Auto-Spin and Turbo Mode

### Auto-Spin

Auto-spin lets the player set a number of spins (10, 25, 50, 100, or unlimited) and the game spins automatically without clicking each time. This is a standard slot game feature that players expect.

The implementation:
- Player selects the number of auto-spins and clicks start.
- After each spin completes (including all animations), the next spin fires automatically.
- Auto-spin stops when: the count is reached, the balance drops below the bet amount, a bonus round triggers, or the player manually stops.
- Show a counter ("Auto: 7/25") and a stop button.

### Turbo Mode

Turbo mode speeds up the animations. Instead of watching reels spin for 2 seconds each, they snap into place quickly. Cascade animations are faster. Win celebrations are shorter.

Turbo doesn't change the game logic or outcomes at all. It only affects animation timing. Some players want to watch every spin unfold dramatically. Others want to grind through spins quickly. Turbo mode serves the second group.

The implementation is straightforward: define two sets of animation durations (normal and turbo) and toggle between them based on the turbo state.

### When Directing Claude Code

> "Claude, add auto-spin and turbo mode to Frat House Frenzy. Auto-spin: let the player choose 10/25/50/100/unlimited spins, show a counter, auto-fire next spin after animations complete, stop on count reached, low balance, bonus trigger, or manual stop. Turbo mode: a toggle that reduces all animation durations by 60-70%. Both should be toggleable from the BetControls component. Store the settings in the game's UI state."

---

## What's Next

Head to [project.md](project.md) to build the reel UI, add animations and sound, create the admin dashboard, and ship v1 of Frat House Frenzy.
