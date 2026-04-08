# Project: Frat House Frenzy -- Wallet & Payments

**What you're building:** Adding Stripe-powered deposits and withdrawals to Frat House Frenzy, a bet placement system, balance management, transaction history, and bonus buy feature gating by payment tier.

**Time estimate:** 10-12 hours

**What you'll need open:** Ghostty (your terminal), a web browser, the Stripe dashboard (test mode), and the Stripe CLI.

---

## The Brief

Frat House Frenzy has player accounts, balances, sessions, and spin records. But players can't put money in or take money out. This week you'll add the payment infrastructure: Stripe for deposits and withdrawals, a proper bet placement system with configurable bet sizes, transaction history, and the bonus buy feature -- letting players pay 80x/200x/500x/69x their bet to skip straight into bonus rounds. The bonus buy tiers map directly to feature gating by payment level.

After this week, Frat House Frenzy can handle real money. You'll work in test mode, but the infrastructure is production-ready.

---

## Phase 1: Research

Before building, understand the tools:

> "Claude, I need to add Stripe deposits and withdrawals to my Next.js slot game. Players deposit money to get a balance, place bets, and withdraw winnings. What's the right approach? Should I use Stripe Checkout for deposits? How do I handle withdrawals (payouts) through Stripe?"

> "Claude, how should I sync Stripe payments with my player balance? Should I credit the balance immediately when the player pays, or wait for the webhook confirmation? What's the safest approach to avoid crediting deposits that don't actually complete?"

> "Claude, what happens when a deposit payment fails? How does Stripe handle retries? How should my app behave if a player tries to play while a deposit is still processing?"

> "Claude, how do I test Stripe webhooks locally? Walk me through the Stripe CLI setup."

### Set Up Stripe

Before writing any code:

1. Create a Stripe account at [stripe.com](https://stripe.com) if you don't have one
2. Make sure you're in **test mode** (toggle in the top-right of the dashboard)
3. Install the Stripe CLI: ask Claude for the install command for your OS
4. Note: you won't need subscription products -- deposits are one-time payments

> "Claude, walk me through setting up Stripe for one-time deposits in test mode. I need to handle variable deposit amounts, not fixed subscription prices."

---

## Phase 2: Plan

> "Claude, plan the Stripe integration for Frat House Frenzy. Here's what I need:
>
> **Deposits (via Stripe Checkout):**
> - Player chooses a deposit amount ($5, $10, $25, $50, $100, or custom)
> - Redirect to Stripe Checkout for payment
> - On successful payment, credit the player's balance
> - Record the deposit in balance_history
>
> **Withdrawals:**
> - Player requests a withdrawal from their balance
> - Minimum withdrawal: $10
> - Deduct from balance immediately, mark withdrawal as pending
> - Admin approval flow (simple: admin marks as approved/rejected)
> - If rejected, refund the balance
> - Record in balance_history
>
> **Webhooks:**
> - Endpoint at /api/webhooks/stripe
> - Handle: checkout.session.completed, payment_intent.succeeded, payment_intent.payment_failed
> - Signature verification, idempotency (skip duplicate events)
> - Only credit balance after webhook confirmation (not on redirect)
>
> **Bet Placement System:**
> - Configurable bet sizes: $0.10, $0.25, $0.50, $1.00, $2.50, $5.00, $10.00, $25.00, $50.00, $100.00
> - Quick bet buttons and a custom amount input
> - Validate bet amount with Zod: positive, within allowed range, within player balance
> - Bet amount persists across spins until the player changes it
>
> **Bonus Buy Feature:**
> - Four bonus buy options mapped to bet multipliers:
>   - Party Mode: 80x bet (guaranteed 3 scatters)
>   - Darty Mode: 200x bet (guaranteed 4 scatters)
>   - Full Send Mode: 500x bet (guaranteed 5 scatters)
>   - Random Mode: 69x bet (random bonus tier)
> - Each bonus buy deducts the multiplied amount from balance
> - Gate bonus buy options by minimum balance (must afford the buy)
> - This is feature gating by payment tier -- same pattern as SaaS plan tiers
>
> **Transaction History:**
> - Full transaction log at /wallet showing deposits, withdrawals, bets, and wins
> - Filter by type, date range
> - Running balance column
>
> **Database changes:**
> - deposits table (id, playerId, amount, stripeSessionId, stripePaymentIntentId, status: pending/completed/failed, createdAt)
> - withdrawals table (id, playerId, amount, status: pending/approved/rejected/completed, requestedAt, processedAt, processedById)
>
> Plan the implementation order, database changes, API routes, and UI updates."

Review the plan. Key questions:

- Is the balance only credited after webhook confirmation (never on the redirect alone)?
- What happens if a player deposits and the webhook is delayed? Can they still play?
- How are withdrawals protected against race conditions (player withdrawing more than their balance)?
- Are bonus buy deductions atomic with the balance check?
- Are all monetary values stored as integer cents?

---

## Phase 3: Execute

### Step 1: Environment Setup

> "Claude, add Stripe environment variables: STRIPE_SECRET_KEY, STRIPE_PUBLISHABLE_KEY, STRIPE_WEBHOOK_SECRET. Update .env.local and .env.example. Install the stripe npm package."

### Step 2: Database Changes

> "Claude, add the payment tables:
> 1. deposits table: id, playerId (references player_profiles), amount (integer cents), stripeSessionId (string, unique), stripePaymentIntentId (string, nullable), status (enum: pending/completed/failed, default 'pending'), createdAt
> 2. withdrawals table: id, playerId (references player_profiles), amount (integer cents), status (enum: pending/approved/rejected/completed, default 'pending'), requestedAt, processedAt (nullable), processedById (nullable, references users)
> Run the migration."

### Step 3: Deposit Flow

> "Claude, build the deposit flow:
> 1. Deposit page at /wallet/deposit with preset amounts ($5, $10, $25, $50, $100) and a custom amount input
> 2. Create an API route that creates a Stripe Checkout Session for the selected amount
> 3. Include the playerId and deposit record ID in the session metadata
> 4. Set success_url to /wallet?deposit=success and cancel_url to /wallet?deposit=canceled
> 5. Create a pending deposit record in the database before redirecting to Stripe
> 6. Style the deposit page with the dark theme -- neon green accent on amount buttons, dark card backgrounds
> Validate the deposit amount with Zod: minimum $5, maximum $500, must be a positive number."

Test:
- Click a deposit amount
- You should be redirected to Stripe's checkout page
- Use test card `4242 4242 4242 4242`, any future expiry, any CVC
- After payment, you should be redirected back to your app

### Step 4: Webhook Handler

Start the Stripe CLI to forward webhooks locally:

```
stripe listen --forward-to localhost:3000/api/webhooks/stripe
```

Copy the webhook signing secret it displays.

> "Claude, build the webhook handler at /api/webhooks/stripe:
> 1. Verify the Stripe signature using the webhook secret
> 2. Parse the event type
> 3. Check for duplicate events (store processed event IDs or use idempotency keys)
> 4. Handle these events:
>    - checkout.session.completed: look up the deposit from metadata, update status to 'completed', credit the player's balance, create a balance_history entry with reason 'deposit'
>    - payment_intent.payment_failed: look up the deposit, update status to 'failed'
> 5. All balance credits must happen inside a database transaction
> 6. Return 200 for all events (even unhandled ones -- Stripe retries on non-200)
> 7. Log every event received with type and player ID"

Test the full flow:
- Complete a deposit with the test card
- Check the Stripe CLI output -- you should see the webhook events
- Check your database -- the player's balance should be updated
- Refresh the wallet page -- the balance should reflect the deposit
- Check balance_history -- there should be a deposit entry

### Step 5: Withdrawal System

> "Claude, build the withdrawal system:
> 1. Withdrawal request form at /wallet/withdraw
> 2. Player enters an amount (minimum $10, maximum their current balance)
> 3. On submit: deduct from balance immediately, create a pending withdrawal record, create a balance_history entry with reason 'withdrawal'
> 4. The deduction and withdrawal creation must be atomic (database transaction)
> 5. Show pending withdrawals on the wallet page with status badges
> 6. Admin route at /admin/withdrawals showing all pending withdrawal requests
> 7. Admin can approve or reject each request
> 8. If rejected: refund the amount back to the player's balance, create a balance_history entry with reason 'adjustment'
> Validate with Zod: amount must be positive, at least $10, not exceed current balance."

Test:
- Request a withdrawal
- Verify balance is deducted immediately
- Check the admin page -- is the request listed?
- Approve it -- verify status updates
- Reject a different request -- verify balance is refunded

### Step 6: Bet Placement System

> "Claude, build the bet placement controls:
> 1. Bet selector component with preset buttons: $0.10, $0.25, $0.50, $1.00, $2.50, $5.00, $10.00, $25.00, $50.00, $100.00
> 2. Custom bet input field with Zod validation
> 3. The selected bet amount persists in local storage across page reloads
> 4. Bet amount cannot exceed current balance -- disable amounts that are too high
> 5. Quick buttons: 'Min Bet', 'Max Bet', 'Double', 'Halve'
> 6. Display the potential payout range based on the current bet (using placeholder multipliers for now)
> 7. Style with the dark theme: selected bet button glows neon green, disabled buttons are dimmed
> Place this component below the game grid on the /play page."

Test:
- Select different bet amounts
- Verify amounts above your balance are disabled
- Use the quick buttons (min, max, double, halve)
- Reload the page -- does the bet amount persist?

### Step 7: Bonus Buy Feature

> "Claude, build the bonus buy feature:
> 1. Bonus buy panel on the /play page (below or beside the bet controls)
> 2. Four bonus buy options displayed as cards:
>    - Party Mode (80x bet): 'Guaranteed 3 scatters -- 8 free spins'
>    - Darty Mode (200x bet): 'Guaranteed 4 scatters -- 12 spins, 5x5 grid'
>    - Full Send Mode (500x bet): 'Guaranteed 5 scatters -- 12 spins, 5x6 grid, 3x multiplier'
>    - Random Mode (69x bet): 'Random bonus tier -- feeling lucky?'
> 3. Each card shows the cost based on current bet amount (e.g., at $1.00 bet, Party Mode costs $80.00)
> 4. Gate each option: if cost exceeds balance, show the card as locked with a 'Deposit to unlock' prompt
> 5. Clicking an unlocked bonus buy deducts the cost from balance and records it as a special spin
> 6. This is the same pattern as SaaS feature gating -- users at different balance tiers access different features
> Style the cards with neon borders. Locked cards are dimmed with a lock icon. Unlocked cards glow."

Test:
- With a low balance, verify some bonus buys are locked
- Deposit enough to unlock all tiers
- Purchase a bonus buy -- verify the correct amount is deducted
- Verify balance_history records the bonus buy

### Step 8: Transaction History

> "Claude, enhance the wallet page at /wallet:
> 1. Current balance displayed prominently at the top with neon green text
> 2. Quick action buttons: 'Deposit' and 'Withdraw'
> 3. Transaction history table showing all balance changes: date, type (deposit/withdrawal/bet/win/bonus_buy), description, amount (+/-), balance after
> 4. Color code: green for credits, red for debits
> 5. Filter by transaction type and date range
> 6. Pagination -- 25 transactions per page with 'Load More'
> 7. Show pending deposits and withdrawals with status badges"

### Step 9: Quality Gates

> "Claude, run npm run lint and fix any issues."

> "Claude, run npm run typecheck and fix any issues."

> "Claude, run npm run build and make sure it completes without errors."

### Step 10: Deploy

> "Claude, commit all changes, push to GitHub, and deploy to Vercel. Add the Stripe environment variables (STRIPE_SECRET_KEY, STRIPE_PUBLISHABLE_KEY, STRIPE_WEBHOOK_SECRET) to Vercel."

After deploying, set up the production webhook:

> "Claude, walk me through creating a webhook endpoint in the Stripe dashboard that points to my production URL at /api/webhooks/stripe. What events should I subscribe to?"

Test on the live URL:
- Complete a full deposit flow with test card
- Check the Stripe dashboard for the payment
- Verify balance updates after the webhook fires
- Place some bets with different amounts
- Try a bonus buy
- Request a withdrawal
- Check the full transaction history
- Verify feature gating on bonus buys works correctly

---

## Acceptance Criteria

Your project is complete when all of these are true:

- [ ] Stripe is integrated in test mode for deposits
- [ ] Deposit flow works (preset amounts, redirect to Stripe, balance credited via webhook)
- [ ] Webhook handler verifies signatures and handles duplicates
- [ ] Balance is only credited after webhook confirmation (not on redirect)
- [ ] Withdrawal requests can be submitted and appear in admin queue
- [ ] Admin can approve or reject withdrawals
- [ ] Rejected withdrawals refund the player's balance
- [ ] Bet placement controls work with preset and custom amounts
- [ ] Bet validation enforced in UI (disable amounts exceeding balance) and API (reject overspending)
- [ ] Bonus buy feature shows four tiers with correct cost calculations
- [ ] Bonus buys gated by balance (locked if too expensive, with deposit prompt)
- [ ] Transaction history shows all balance changes with filtering
- [ ] All monetary values stored as integer cents
- [ ] All balance mutations use database transactions
- [ ] `npm run lint` passes
- [ ] `npm run typecheck` passes
- [ ] `npm run build` completes without errors
- [ ] Code is on GitHub and deployed to Vercel

---

## Stretch Goals

**Add Deposit Bonuses**
> "Claude, add a deposit bonus system. First deposit gets a 100% match bonus (deposit $50, get $50 bonus). The bonus has a 30x wagering requirement before it can be withdrawn. Track bonus balance separately from real balance. Show wagering progress on the wallet page."

**Add a VIP Tier System**
> "Claude, add VIP tiers based on total wagered: Bronze (0-$500), Silver ($500-$5,000), Gold ($5,000-$25,000), Platinum ($25,000+). Each tier unlocks perks: better deposit bonuses, faster withdrawals, exclusive bonus buy discounts. Show the VIP tier and progress on the profile page."

**Add Recurring Deposits**
> "Claude, add an auto-reload feature. Players can set a threshold (e.g., 'when balance drops below $5, auto-deposit $25'). Use Stripe's saved payment methods. Let players enable/disable this from the wallet page."

**Add Cashback Rewards**
> "Claude, add a cashback system. Players earn 0.5% cashback on all bets. Cashback accumulates and can be claimed once it reaches $1.00. Show pending cashback on the wallet page. This is the same pattern as loyalty points in SaaS products."

---

## Troubleshooting

**Deposit completes but balance doesn't update**
> "Claude, I completed a deposit on Stripe but the player's balance didn't update. Check: is the Stripe CLI running and forwarding webhooks? Is the webhook URL correct? Is the signing secret correct? Is the webhook handler actually crediting the balance inside a transaction? Check the Stripe CLI logs and the server logs."

**Webhook signature verification fails**
> "Claude, the webhook handler returns a signature error. Check: are you using the correct webhook secret (the one from `stripe listen`, not from the Stripe dashboard)? Is the raw request body being passed to the verification function (not a parsed JSON body)?"

**Balance goes negative after a bonus buy**
> "Claude, a player's balance went negative after a bonus buy. The balance check and deduction are not atomic. Wrap the check and deduction in a database transaction with a SELECT FOR UPDATE on the balance row to prevent race conditions."

**Withdrawal approved but balance not refunded on rejection**
> "Claude, I rejected a withdrawal but the player's balance wasn't restored. Check the rejection handler -- is it creating a balance_history entry with reason 'adjustment' and adding the amount back to the balance? Is this happening inside a transaction?"

**Bonus buy costs showing wrong amounts**
> "Claude, the bonus buy cards show incorrect costs. Check that the cost calculation multiplies the current bet amount (in cents) by the correct multiplier (80x, 200x, 500x, 69x) and displays it formatted as dollars. Watch for floating point errors -- do all math in cents."

---

## Reflection

You just added a payment system to a gaming platform. Think about what that means: real deposits through Stripe, balance management with full audit trails, withdrawal processing with admin approval, and feature gating based on what players can afford.

Notice the pattern: Stripe handles the hard parts (payment processing, payment UI, compliance). Your app handles the business logic (crediting balances, enforcing bet limits, gating bonus buys by affordability). This division of responsibility -- letting specialized services handle specialized problems -- is the core principle of modern software architecture.

The bonus buy feature is particularly interesting. It's the exact same pattern as SaaS plan tiers: different features unlock at different price points. Whether you're gating "unlimited projects" behind a $12/month plan or gating "Full Send Mode" behind a 500x bet, the code pattern is identical.

Next week, you'll build the actual game engine: RNG, provably fair mechanics, and the math model.
