# Project: Frat House Frenzy -- Foundation

**What you're building:** A provably fair slot game with player authentication, a PostgreSQL database for player data (balances, game history, sessions), Zod-validated bet inputs, and a dark-themed Next.js frontend with neon accents.

**Time estimate:** 12-15 hours

**What you'll need open:** Ghostty (your terminal), a web browser, and your Neon database dashboard.

---

## The Brief

You're building a real game product. Not a toy, not a demo -- a provably fair slot game called Frat House Frenzy that players could actually deposit into, spin, and verify every outcome. This week is the foundation: player accounts, the database layer for balances and game history, basic CRUD operations, and the visual shell. Weeks 9-11 add payments, the game engine, and the reel UI.

Think of this as building an online slot platform like Stake or Roobet. Simpler, but the same architecture.

---

## Phase 1: Research

Start Claude Code and explore the problem space:

> "Claude, I'm building a provably fair online slot game called Frat House Frenzy. It needs player accounts with authentication, a database to track balances, game sessions, bet history, and spin results. What's the best data model for a slot game backend? How should I structure the database relationships between players, sessions, bets, and results?"

> "Claude, how does Better Auth handle user accounts for a gaming platform? I need email/password signup and session management. How do I protect routes so only logged-in players can access the game?"

> "Claude, what are the most important data operations for a slot game? I need to track player balances, record every bet, store spin results, and maintain a game history log. What status workflows do game sessions typically have?"

> "Claude, how should I set up a dark-themed Next.js app with Tailwind? I want a dark background with neon accent colors -- think gritty party lighting. What's the best approach for Tailwind dark mode with custom neon color tokens?"

---

## Phase 2: Plan

> "Claude, plan the full build for Frat House Frenzy's foundation. Here's the spec:
>
> **Auth:**
> - Better Auth with email/password
> - Session management, protected routes
> - Players must be logged in to access the game
>
> **Player Profiles:**
> - Every authenticated user gets a player profile
> - Display name, avatar placeholder, join date
> - Profile settings page
>
> **Balance System:**
> - Each player has a balance (stored as integer cents to avoid floating point)
> - Balance starts at 0 (deposits come in Week 9)
> - Balance history tracks every credit and debit with a reason
>
> **Game Sessions:**
> - A session represents a play period (player sits down to play)
> - Track session start, end, total bets, total wins, number of spins
> - Sessions have status: active/completed
>
> **Bet History:**
> - Every spin is recorded: bet amount, result, payout, timestamp
> - Linked to both the player and the session
> - Status: pending/settled/void
>
> **Dark Theme Shell:**
> - Tailwind dark mode enabled by default
> - Custom color tokens: neon green (#39FF14), neon pink (#FF6EC7), neon blue (#04D9FF), dark backgrounds (#0a0a0a, #1a1a1a)
> - Layout: nav bar with player balance display, main game area, footer
> - Placeholder game area with 'Coming Soon' state
>
> **Monitoring:**
> - Sentry for errors, PostHog for analytics
>
> Plan the database schema, API routes, page structure, and component breakdown. Use Next.js, TypeScript, Tailwind, shadcn/ui, Drizzle ORM, Postgres on Neon, Zod for validation."

Review the plan carefully. Key questions:

- Does the database schema properly link players to sessions, sessions to bets?
- Are balances stored as integers (cents) to avoid floating point errors?
- How does the balance history maintain an audit trail?
- What does the URL structure look like?
- Are there proper foreign key constraints?

---

## Phase 3: Execute

### Step 1: Project Setup

> "Claude, create the Next.js project for Frat House Frenzy with TypeScript, Tailwind, shadcn/ui, and Drizzle ORM configured for Neon Postgres. Set up Tailwind with dark mode enabled by default and custom neon color tokens: neonGreen (#39FF14), neonPink (#FF6EC7), neonBlue (#04D9FF). The default background should be near-black (#0a0a0a). Set up the CLAUDE.md with project conventions, including the rule: every balance mutation must create a balance_history record."

> "Claude, set up the environment variables: DATABASE_URL, BETTER_AUTH_SECRET, SENTRY_DSN, NEXT_PUBLIC_POSTHOG_KEY. Create .env.local and .env.example."

### Step 2: Database Schema

> "Claude, create the full database schema with Drizzle ORM:
> - users (managed by Better Auth)
> - player_profiles (id, userId, displayName, avatarUrl, createdAt)
> - balances (id, playerId, amount as integer in cents, updatedAt)
> - balance_history (id, playerId, amount, balanceBefore, balanceAfter, reason: deposit/withdrawal/bet/win/bonus/adjustment, referenceId, createdAt)
> - game_sessions (id, playerId, status: active/completed, startedAt, endedAt, totalBets, totalWins, spinCount)
> - spins (id, sessionId, playerId, betAmount, payout, result as jsonb, status: pending/settled/void, createdAt)
>
> Add proper foreign keys, indexes on playerId and sessionId, and constraints. The balance amount must never go below zero (add a check constraint). Run the migration."

Verify the database:

> "Claude, show me the migration SQL that was generated. I want to verify the foreign keys and constraints are correct, especially the non-negative balance constraint."

### Step 3: Authentication

> "Claude, integrate Better Auth with email/password. Create login and signup pages with shadcn/ui using the dark theme -- dark card backgrounds, neon green accent on buttons, white text. After signup, automatically create a player_profile and a balance record (starting at 0). Redirect new users to the game lobby."

Test thoroughly:
- Sign up with email and password
- Log out and log back in
- Sign up with a second account (use a different email)
- Verify a player_profile and balance record were created for each user

### Step 4: Player Profile Management

> "Claude, build the player profile system:
> 1. After first login, show the game lobby if the player profile exists
> 2. Profile page at /profile showing display name, join date, and play stats (total sessions, total spins, biggest win -- all zeros for now)
> 3. Edit profile form (display name)
> 4. Player balance displayed prominently in the nav bar with a neon green glow effect
> 5. Balance formatted as currency ($0.00)
> All data queries must be scoped to the current player."

Test:
- View your profile
- Update your display name
- Verify the balance shows $0.00 in the nav bar

### Step 5: Game Session Management

> "Claude, build the game session system:
> 1. When a player enters the game area, create a new active session (or resume the existing active session)
> 2. Session detail view showing: duration, number of spins, total wagered, total won, net result
> 3. End session button that sets status to completed and records the end time
> 4. Session history page at /history showing past sessions with date, duration, spins, and net result
> 5. Only one active session per player at a time
> Validate all inputs with Zod."

Test:
- Start a session
- Verify it shows as active
- End the session
- Check the history page

### Step 6: Bet History and Spin Records

> "Claude, build the spin/bet recording system:
> 1. API route to record a spin: accepts betAmount, validates with Zod (must be positive integer, within allowed range 10-10000 cents)
> 2. For now, create a mock spin result (random win/loss) -- the real engine comes in Week 10
> 3. Deduct bet from balance, add any winnings, create balance_history entries for both the bet and the win
> 4. Record the spin in the spins table linked to the active session
> 5. Update session totals (totalBets, totalWins, spinCount)
> 6. Spin history view within a session showing each spin's bet, result, and payout
> All balance operations must be atomic -- use a database transaction."

Test:
- Manually set a test balance (we'll add real deposits in Week 9)
- Place a spin and verify the balance updates
- Check that balance_history has entries for both the bet and any win
- Verify session totals update correctly

### Step 7: Game Lobby and Dark Theme Shell

> "Claude, build the game lobby and visual shell:
> 1. Landing page at / with the Frat House Frenzy logo/title in neon style
> 2. If not logged in: show a dark, atmospheric landing page with sign up / log in CTAs
> 3. If logged in: show the game lobby with player balance, a 'Play Now' button, and recent session history
> 4. Game page at /play with a placeholder 5x4 grid (dark cards with neon borders) and a bet controls area
> 5. The grid should show placeholder symbol slots with a 'Coming Soon' overlay
> 6. Bet controls: bet amount selector (disabled until Week 10), spin button (disabled), and balance display
> Make it look atmospheric -- dark backgrounds, subtle gradients, neon glows on interactive elements. Use shadcn/ui components styled with the dark theme."

### Step 8: Balance Display and Validation

> "Claude, build robust balance display and validation:
> 1. Real-time balance display in the nav bar that updates after every spin
> 2. Balance page at /wallet showing current balance and full transaction history
> 3. Transaction history table: date, type (bet/win/deposit/withdrawal), amount, balance after
> 4. Zod validation on all bet inputs: betAmount must be a positive integer, within min/max range, and not exceed current balance
> 5. API-level validation: reject any spin where betAmount > current balance
> 6. Optimistic UI update on the balance after a spin, with rollback on error"

Test:
- View the wallet page with transaction history
- Attempt to bet more than the balance -- should be rejected
- Verify Zod validation error messages are clear

### Step 9: Monitoring

> "Claude, add Sentry for error monitoring and PostHog for analytics. Track these events: player-signed-up, session-started, session-ended, spin-placed, big-win (payout > 10x bet), balance-deposited, balance-withdrawn."

### Step 10: Quality Gates

> "Claude, run npm run lint and fix any issues."

> "Claude, run npm run typecheck and fix any issues."

> "Claude, run npm run build and make sure it completes without errors."

### Step 11: Deploy

> "Claude, commit all changes, push to GitHub, and deploy to Vercel. Walk me through setting up the environment variables in Vercel."

Test on the live URL:
- Full signup and login flow
- View profile and update display name
- Check the game lobby with balance display
- Start and end a game session
- View session history and wallet transaction history
- Verify the dark theme and neon accents look correct
- Check Sentry and PostHog dashboards

---

## Acceptance Criteria

Your project is complete when all of these are true:

- [ ] App is live on Vercel with dark theme and neon accents
- [ ] Players can sign up with email/password via Better Auth
- [ ] Player profiles are created automatically on signup
- [ ] Balance system works with integer cents (no floating point)
- [ ] Balance history creates an audit trail for every credit and debit
- [ ] Game sessions can be started, tracked, and completed
- [ ] Spin records are stored with bet amount, result, and payout
- [ ] All balance mutations happen inside database transactions
- [ ] Wallet page shows current balance and transaction history
- [ ] Bet input validation with Zod (positive integer, within range, within balance)
- [ ] Game lobby shows placeholder 5x4 grid with dark theme
- [ ] Nav bar displays player balance with neon styling
- [ ] Sentry and PostHog are integrated
- [ ] `npm run lint` passes
- [ ] `npm run typecheck` passes
- [ ] `npm run build` completes without errors
- [ ] Code is on GitHub

---

## Stretch Goals

**Add Player Avatars**
> "Claude, add avatar upload to player profiles using Vercel Blob. Let players upload a profile image that displays in the nav bar and on the game lobby. Show a default avatar placeholder for players who haven't uploaded one."

**Add a Leaderboard**
> "Claude, add a leaderboard page showing top players by biggest single win, most total wins, and longest session. Use proper database aggregation queries. Update in near-real-time."

**Add Achievement Badges**
> "Claude, add an achievements system. Track milestones like 'First Spin', '100 Spins', 'First Big Win (10x+)', 'High Roller (bet max)'. Store achievements per player and display them on the profile page with badge icons."

**Add a Session Replay Log**
> "Claude, add a detailed spin-by-spin replay view for completed sessions. Show each spin's bet, the grid result, payout, and running balance -- like a hand history in poker."

---

## Troubleshooting

**Balance going negative**
> "Claude, a player's balance went below zero. Audit the spin API route to make sure it checks the balance BEFORE deducting the bet, and that the check and deduction happen inside the same database transaction. This is a critical integrity issue."

**Player profile not created on signup**
> "Claude, a user signed up but has no player_profile or balance record. Check the signup handler -- is it creating the profile and balance after Better Auth creates the user? Is the creation wrapped in a try/catch so we can see errors?"

**Session not ending properly**
> "Claude, the game session stays active even after clicking End Session. Check the session update API route. Is it setting the status to 'completed' and recording the endedAt timestamp? Is the UI reading the updated status?"

**Dark theme not applying everywhere**
> "Claude, some pages or components are showing white backgrounds instead of the dark theme. Check that Tailwind's dark mode is set to 'class' or 'media' consistently, and that all components use dark-appropriate background and text colors. Audit shadcn/ui component overrides."

---

## Reflection

You just built the foundation for a real gaming platform. Think about what that means: player accounts, a balance system with full audit trails, game sessions, and bet records -- all with proper data integrity using database transactions.

The data model you designed this week -- players, balances, sessions, spins, and balance history -- is the backbone of any online gaming product. Whether it's slots, poker, or sports betting, the architecture is the same: authenticate the player, track every cent, record every action, and never let the balance go negative.

Next week, you'll add real money: Stripe for deposits and withdrawals, and the bet placement system.
