# Week 8 Guide: Frat House Frenzy — Foundation

Your bookmark manager serves one person. Your portfolio is a personal site. Now you're building something that serves many people, playing a game, managing balances, tracking wins and losses. That shift -- from personal tool to interactive game with real money mechanics -- is what separates a project from a product.

---

## Part 1: What a Web Game Needs vs a SaaS App

You've been building tools. Now you're building entertainment. The mental model changes.

A SaaS app like a project manager asks: "Which organization does this belong to?" A game asks: "What is this player's balance, and what happens when they press Spin?"

Three things define an online slot game:

**Player accounts.** Every player has their own account, their own balance, their own spin history. Authentication works the same way it does in a SaaS -- Better Auth handles it -- but the context is different. Instead of workspaces and teams, you have individual players with wallets.

**Game state.** At any moment, the game needs to know: What is the player's balance? Are they in the middle of a bonus round? What's the current multiplier? Game state is more volatile than SaaS state. A task in a project manager might sit unchanged for days. A slot game's state changes every second during play.

**Balance management.** This is the most critical difference. In a SaaS, if a feature-gate check fails, the worst case is a user sees something they shouldn't. In a game with money, if a balance check fails, real money is lost or created out of thin air. Every cent must be accounted for, every transaction must be atomic, every calculation must be exact.

### The Game Mental Model

When you're directing Claude Code to build Frat House Frenzy, think about your app as serving individual players, each with their own wallet and game history. Every feature you add needs to ask: "What does this cost the player?" and "What could this pay the player?" Every database operation that touches money must be wrapped in a transaction.

This is the biggest conceptual shift from what you've built so far. Your bookmark manager asked "which user?" Frat House Frenzy asks "which player, with what balance, in what game state, and is this operation financially safe?"

---

## Part 2: Player Authentication with Better Auth

Good news: authentication works the same way regardless of what you're building. Better Auth handles player accounts just like it handled user accounts in your earlier projects. The difference is context, not technology.

### What Changes for a Game

**Registration feels different.** A SaaS signup asks for company details, team size, use case. A game signup should be fast and frictionless -- email, password, done. Players want to play, not fill out forms. The fewer fields, the better.

**Sessions matter more.** In a SaaS, if a user's session expires mid-task, they log in again and pick up where they left off. In a game, if a session expires mid-spin, you need to handle the in-progress game state. What happens to their bet? Was the spin resolved? Session management must account for active gameplay.

**Display names vs real names.** SaaS users often use their real names. Game players often want a username or display name. Your player profile should support this.

### When Directing Claude Code

> "Claude, set up Better Auth for player authentication. Keep registration minimal -- email, password, and a display name. Configure session handling so active game states are preserved if a session refreshes. Use the same Better Auth patterns from our earlier projects."

---

## Part 3: Database Design for a Game

The database is where everything lives. Players, balances, game sessions, spin results. Get the schema right and everything else flows. Get it wrong and you'll fight the database for the rest of the project.

### The Core Tables

**Players.** Each player has an account. This extends the auth user with game-specific fields: display name, current balance, account status (active, suspended), and creation date. The player table is the anchor -- everything else connects to it.

**Balances -- Store as Integer Cents.** This is critical. Never store money as floating-point numbers. Computers can't represent all decimal numbers precisely. `0.1 + 0.2` in JavaScript equals `0.30000000000000004`, not `0.3`. If your player has $10.00, store it as `1000` (cents). A $0.50 bet is `50`. A $125.75 win is `12575`.

Every calculation happens in integers. Every display converts cents to dollars at the last possible moment. This is how Stripe works, how banks work, and how your game must work.

**Game sessions.** A game session tracks a player's continuous play period. When did they start playing? When did they stop? What was their starting balance? Ending balance? Sessions are useful for analytics, responsible gaming features, and debugging.

**Spin history.** Every single spin must be recorded. The bet amount, the result (symbol positions), the payout, the timestamp, and the provably-fair seeds used to generate the outcome. This is your audit trail. If a player disputes a result, you can prove exactly what happened.

### Relationships

Think of it as a tree:

```
Player
  └── Balance (stored as integer cents on the player record)
  └── Game Session
        └── Spin (with bet, result, payout, seeds)
```

A player has many game sessions. A game session has many spins. Each spin records the exact bet amount, the outcome, and the payout -- all in integer cents.

### Why This Matters for Directing Claude Code

When you describe this data model to Claude Code, be explicit about the integer-cents rule and the relationships. "Balances are stored as integers representing cents. A spin belongs to a game session. A game session belongs to a player. Every spin records the bet amount, result, and payout as integers." These constraints prevent the two worst bugs in a game: incorrect balances and missing audit records.

### When Directing Claude Code

> "Claude, design the database schema for Frat House Frenzy. The core tables are: players (extending auth users with display name and balance in integer cents), game_sessions (tracking play periods with start/end times and start/end balances), and spin_history (recording every spin with bet amount, symbol results, payout, and provably-fair seeds). All monetary values are integers representing cents. Use Drizzle ORM with proper foreign keys and constraints. Add an index on spin_history by player and timestamp for fast lookups."

---

## Part 4: Drizzle ORM for the Game Schema

You used Drizzle in earlier weeks. The concepts are the same -- define your schema in TypeScript, let Drizzle generate SQL migrations, use the query builder for type-safe database access. What's new is the schema itself and the patterns you'll need for a game.

### Integer Columns for Money

Drizzle supports integer columns. Use them for every monetary field:

- `balance` on the player table: integer, default 0
- `betAmount` on the spin table: integer, not null
- `payoutAmount` on the spin table: integer, not null
- `depositAmount`, `withdrawalAmount` on transaction records: integer, not null

When you tell Claude Code to create these columns, specify "integer, representing cents" so there's no ambiguity.

### Enums for Game States

Games have more states than SaaS apps. A spin can be `pending`, `resolved`, or `error`. A bonus round can be `active`, `completed`, or `forfeited`. A game session can be `active` or `ended`. Use PostgreSQL enums (Drizzle supports them) to enforce valid states at the database level.

### Timestamps Everywhere

In a SaaS, timestamps are nice to have. In a game, they're essential. Every spin needs a timestamp. Every balance change needs a timestamp. Every session start and end needs a timestamp. When something goes wrong -- and in a game with money, things will go wrong -- timestamps are how you reconstruct what happened.

### When Directing Claude Code

> "Claude, implement the Drizzle schema for Frat House Frenzy. All monetary values use integer columns representing cents. Use PostgreSQL enums for game states (spin status, session status, transaction type). Add createdAt timestamps to every table. Make sure the schema enforces referential integrity -- a spin can't exist without a game session, a game session can't exist without a player."

---

## Part 5: Dark Theme and Visual Design

Frat House Frenzy is a dark-comedy slot game set at a chaotic frat party. The visual design isn't decoration -- it's the product. A casino game in a white corporate theme would feel wrong. The theme creates immersion, and immersion is what keeps players engaged.

### Why Dark Theme Matters for a Casino Game

Every real casino -- physical or online -- uses dark backgrounds. There's a reason. Dark backgrounds make bright colors pop. Neon accents glow against black. Winning animations feel more dramatic. The entire visual language of gaming is built on contrast: dark canvas, bright action.

Tailwind CSS makes this straightforward. You already used Tailwind in earlier projects. Now you'll use its dark mode utilities intentionally, not just as an accessibility option.

### The Visual Language

**Background:** Near-black or deep purple. Think of a dark room lit by neon signs and phone screens.

**Accents:** Neon pink, electric blue, toxic green, gold for wins. These are your signal colors -- they draw the eye to important elements like the spin button, the balance display, and win amounts.

**Typography:** Bold, slightly irreverent. The game's personality comes through in the fonts and text treatment. Win amounts should be large and celebratory. Bet amounts should be clear and readable.

**Animation areas:** Leave space in your design for animations. Reels spin. Wins cascade. Multipliers escalate. The UI needs to accommodate motion without feeling cramped.

### When Directing Claude Code

> "Claude, set up the Tailwind theme for Frat House Frenzy. Force dark mode (not system-preference based). Background colors should be near-black with deep purple accents. Add custom colors for neon-pink, electric-blue, toxic-green, and gold. The overall aesthetic is a dark room lit by neon party lights. All text should be high-contrast for readability against dark backgrounds."

---

## Part 6: Game Sessions vs SaaS Workspaces

In the SaaS world, a "workspace" is a persistent container. You create a Slack workspace and it exists forever (or until you delete it). People come and go, but the workspace persists.

A game "session" is temporary. It starts when a player begins playing and ends when they stop. It tracks what happened during that period of play, then it's done. A new play period is a new session.

### Why Sessions Matter

**Responsible gaming.** Sessions let you track how long someone has been playing continuously. If a player has been spinning for three hours straight, you can show a reminder. This is both ethical and, in many jurisdictions, legally required.

**Analytics.** Sessions tell you how players use the game. Average session length, average number of spins per session, win/loss patterns per session. These metrics help you understand your players and improve the game.

**Debugging.** When a player reports a problem -- "I made a deposit but my balance didn't update" or "I won a bonus round but didn't get paid" -- the session gives you a complete timeline of what happened. Every spin, every bet, every payout, in order.

### When Directing Claude Code

> "Claude, implement game sessions. A session starts when a player places their first bet (or explicitly opens the game). It ends when they close the game, when they've been inactive for 15 minutes, or when they explicitly cash out. Track session start time, end time, starting balance, ending balance, total bets, total wins, and number of spins. Link every spin record to its session."

---

## Part 7: The Game Layout

Unlike a SaaS app with navigation menus, sidebars, and content areas, a slot game has a very specific layout. Getting the layout right is essential because players need to see everything that matters at a glance.

### What the Screen Shows

**The reel area.** This is the center of the screen -- the 5x4 grid where symbols appear and cascade. It's the main attraction. Everything else frames it.

**The balance display.** Always visible. Shows the player's current balance in dollars (converted from cents for display). This must update instantly after every spin, win, or deposit.

**The bet controls.** Bet amount selector and the Spin button. The Spin button is the most important interactive element in the entire game. It must be prominent, satisfying to click, and clearly disabled when the player can't spin (insufficient balance, spin in progress).

**The win display.** When a spin wins, the amount appears prominently. Big wins get bigger treatment.

**The game info.** Paytable (what each symbol pays), game rules, and the provably-fair verification link. Usually tucked into a menu or panel so it's accessible but not cluttering the main view.

### When Directing Claude Code

> "Claude, build the main game layout for Frat House Frenzy. Fixed dark background. Center the 5x4 reel grid. Balance display always visible at the top. Bet controls and Spin button at the bottom. Win display overlays the reel area on wins. Add a menu button for paytable and game info. The layout should work on both desktop and mobile. Use the dark theme with neon accent colors."

### Try It: /powerup

As you tackle a larger, more complex project, these intermediate `/powerup` lessons become essential:
- **"Multiply yourself (subagents)"** -- Learn to run parallel agents for complex game work -- one agent building the schema while another scaffolds the UI
- **"Run tasks in background"** -- Keep working while Claude handles longer builds and migrations

---

## What's Next

Head to [project.md](project.md) to build Frat House Frenzy.
