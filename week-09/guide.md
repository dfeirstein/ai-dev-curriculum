# Week 9 Guide: Frat House Frenzy — Wallet & Payments

Money is the hardest integration to get right. Not technically -- Stripe makes the technical part straightforward. The complexity is conceptual: understanding how deposits flow into a game wallet, how bets debit and wins credit that wallet atomically, and making sure every cent is accounted for at all times.

This guide covers the concepts you need to understand before directing Claude Code to add Stripe and wallet management to Frat House Frenzy.

---

## Part 1: How Game Wallets Differ from SaaS Subscriptions

### The Fundamental Difference

A SaaS charges you on a recurring basis. You pay $12/month, you get access to features. The money flows one direction: from customer to business, on a predictable schedule.

A game wallet is a two-way street. Money flows in (deposits), money flows out (withdrawals), and in between, it churns constantly -- bets debit the wallet, wins credit it. The balance changes with every spin. A player might deposit $50, play 200 spins in an hour, and withdraw $73. The transaction volume is orders of magnitude higher than a SaaS.

This means:

**No recurring billing.** Players deposit when they want to play, not on a schedule. There's no "monthly plan." There's no churn rate in the SaaS sense. Instead, you track deposit frequency, average deposit size, and player lifetime value.

**Balance is the product.** In a SaaS, the subscription unlocks features. In a game, the balance IS the feature. Without a balance, the player can't do anything. The deposit flow must be fast, reliable, and frictionless -- any friction between "I want to play" and "I'm playing" loses players.

**Two-way money movement.** SaaS products rarely send money back to customers. Games do, constantly. Withdrawals add complexity: identity verification, fraud prevention, processing delays, and admin approval workflows.

### When Directing Claude Code

> "Claude, the wallet system is fundamentally different from SaaS billing. Players deposit funds (one-time payments), those funds become a balance stored in integer cents, bets debit the balance, wins credit it, and players can withdraw remaining funds. There is no subscription. The balance on the player record is the single source of truth."

---

## Part 2: Stripe for One-Time Deposits

You learned about Stripe Checkout in the context of subscriptions. The same tool handles one-time deposits -- the flow is almost identical, with one key difference: you're creating a payment, not a subscription.

### The Deposit Flow

1. Player clicks "Deposit $25" in the game
2. Your app creates a Stripe Checkout Session for a one-time payment of $25
3. Player is redirected to Stripe's secure checkout page
4. Player enters payment details and confirms
5. Stripe redirects back to your app's success page
6. Stripe sends a webhook confirming the payment succeeded
7. Your app credits the player's wallet with 2500 cents

The critical point: **do not credit the wallet when the player returns to your app.** The redirect URL is not proof of payment. A player could manipulate the URL. Only credit the wallet when you receive and verify the webhook from Stripe. The webhook is Stripe's guarantee that money actually moved.

### Checkout Session Configuration

When creating the Checkout Session, you'll tell Stripe:
- **Mode: `payment`** (not `subscription`) -- this is a one-time charge
- **Amount:** The deposit amount in cents (Stripe also uses cents)
- **Metadata:** The player's ID, so your webhook handler knows which wallet to credit
- **Success URL:** Where to redirect after payment (your game, with a "deposit processing" message)
- **Cancel URL:** Where to redirect if they abandon checkout (back to the deposit page)

### Preset Deposit Amounts

Most games offer preset deposit amounts rather than freeform input. This simplifies the UI and reduces edge cases. Common presets: $10, $25, $50, $100. You can also offer a custom amount field with a minimum ($5) and maximum ($500).

### When Directing Claude Code

> "Claude, implement Stripe deposits using Checkout Sessions in payment mode (not subscription). Offer preset amounts: $10, $25, $50, $100, plus a custom amount field with $5 min and $500 max. Include the player ID in the session metadata. Do NOT credit the player's wallet on redirect -- only credit via the webhook handler after payment confirmation."

---

## Part 3: Webhooks for Confirming Deposits

Webhooks work the same way whether you're building a SaaS or a game. Stripe sends a POST request to your endpoint, you verify it, and you act on it. The difference is what you do when the webhook arrives.

### The Events That Matter

For a game wallet, the critical webhook events are simpler than for a subscription SaaS:

- **`checkout.session.completed`** -- A deposit payment succeeded. Credit the player's wallet.
- **`charge.refunded`** -- A deposit was refunded (by admin or dispute). Debit the player's wallet.
- **`charge.dispute.created`** -- A player filed a chargeback. Flag the account and freeze the balance.

You don't need `invoice.paid`, `customer.subscription.updated`, or any of the subscription lifecycle events. No subscriptions means no subscription webhooks. The event list is shorter, but the handling must be more precise.

### Why Idempotency Matters Even More

Stripe may send the same webhook more than once. In a SaaS, processing a duplicate webhook might create a redundant subscription record -- annoying but fixable. In a game, processing a duplicate deposit webhook credits the player's wallet twice. That's real money appearing from nowhere.

The fix is the same as in any Stripe integration: store the Stripe event ID before processing. Before crediting a wallet, check if you've already processed that event. If yes, skip it. This check must happen inside the same database transaction as the credit.

### Webhook Verification

Anyone could send a POST request to your webhook URL pretending to be Stripe. Verify the webhook signature using Stripe's signing secret before processing. If the signature doesn't match, reject the request immediately. This is not optional.

### When Directing Claude Code

> "Claude, build the Stripe webhook handler for deposits. Verify the Stripe signature first. Check for duplicate events using the event ID. For checkout.session.completed: extract the player ID from metadata, credit their wallet balance, and create a transaction record. For charge.refunded: debit the wallet. For charge.dispute.created: flag the player account. All wallet operations must be inside database transactions."

---

## Part 4: Balance Management — The Atomic Core

This is the most important section in this guide. Balance management is where a game lives or dies. Every operation that changes a player's balance must be atomic -- it either fully succeeds or fully fails, with nothing in between.

### The Three Balance Operations

**Crediting deposits.** When Stripe confirms a deposit, add the amount to the player's balance. This happens in the webhook handler.

**Debiting bets.** When a player clicks Spin, subtract the bet amount from their balance before resolving the spin. If the player has 1000 cents and bets 100 cents, their balance becomes 900 cents before the reels even start spinning.

**Crediting wins.** After the spin resolves and a win is calculated, add the win amount to the player's balance. If that 100-cent spin wins 500 cents, the balance goes from 900 to 1400 cents.

### Why Atomicity Matters

Imagine this sequence without transactions:

1. Player balance: 1000 cents
2. Player clicks Spin (bet: 100 cents)
3. Your app reads the balance: 1000
4. Your app subtracts the bet: 1000 - 100 = 900
5. **The server crashes before writing the new balance**
6. The spin was processed, the bet was deducted from the logical state, but the database still says 1000

Now the player has their original balance AND potentially a win from a spin they "didn't pay for." Or worse, the reverse: the balance is deducted but the spin result is never recorded, so the player loses money without getting to play.

Database transactions prevent this. A transaction groups multiple operations into a single all-or-nothing unit. Either the bet is deducted AND the spin is recorded AND the win is credited, or none of it happens.

### The Spin Transaction

Every spin should execute as a single database transaction:

1. Read the player's current balance
2. Verify the balance is sufficient for the bet
3. Debit the bet amount
4. Generate the spin result (using the provably-fair algorithm)
5. Calculate the payout
6. Credit the payout (if any)
7. Record the spin in history
8. Commit the transaction

If any step fails, the entire transaction rolls back. The player's balance is unchanged. The spin never happened.

### Preventing Race Conditions

What if a player opens two browser tabs and clicks Spin simultaneously? Without protection, both tabs could read the same balance, both could deduct a bet, and the player effectively bets twice on one balance's worth of funds.

The fix: use a database-level lock. When the transaction starts, lock the player's row. The second tab's transaction has to wait until the first one finishes. This is called "SELECT FOR UPDATE" in SQL, and Drizzle supports it.

### When Directing Claude Code

> "Claude, every balance-changing operation must be an atomic database transaction. The spin flow: read balance, verify sufficient funds, debit bet, generate result, calculate payout, credit payout, record spin -- all in one transaction. Use SELECT FOR UPDATE to lock the player row during the transaction to prevent race conditions from concurrent requests. If any step fails, roll back the entire transaction."

---

## Part 5: Withdrawal System with Admin Approval

Players need to be able to take their money out. Withdrawals are more complex than deposits because money is leaving your system, which introduces fraud risk and regulatory requirements.

### The Withdrawal Flow

1. Player requests a withdrawal for a specific amount
2. The amount is immediately deducted from their playable balance and moved to a "pending withdrawal" state
3. An admin reviews the withdrawal request
4. If approved, the funds are transferred to the player (via Stripe payout or manual transfer)
5. If rejected, the funds are returned to the player's playable balance

### Why Admin Approval

Automatic withdrawals are dangerous. A compromised account could drain funds instantly. A player who deposited with a stolen credit card could withdraw before the chargeback arrives. Admin review is a safety net.

The admin dashboard should show pending withdrawals with key information: player account age, deposit history, play history, and the withdrawal amount relative to their deposits. A player who deposited $50, played for 30 minutes, and wants to withdraw $47 is normal. A player who deposited $500 five minutes ago and wants to withdraw $500 without playing is suspicious.

### The Pending Balance Concept

When a withdrawal is pending, the player shouldn't be able to bet with that money. This means you need two concepts of balance:

- **Playable balance:** What the player can bet with
- **Pending withdrawal:** Money reserved for a withdrawal request

The total balance is playable + pending. But only the playable balance is available for spins.

### When Directing Claude Code

> "Claude, implement a withdrawal system. Players request a withdrawal amount, which is deducted from their playable balance and held as pending. Admins see a withdrawal queue with player details (account age, deposit history, play history). Admins approve or reject. Approved withdrawals are processed. Rejected withdrawals return funds to the playable balance. Track all withdrawal state changes with timestamps."

---

## Part 6: Bonus Buy as Feature Gating by Payment

In a SaaS, feature gating means "pay for Pro to unlock premium features." In Frat House Frenzy, feature gating means "pay a premium to skip straight to the bonus round." This is the Bonus Buy mechanic.

### How Bonus Buy Works

Normally, a player reaches the bonus round by landing 3, 4, or 5 scatter symbols -- roughly 1 in 195 spins. Some players don't want to wait. Bonus Buy lets them pay a premium to trigger the bonus immediately.

The Bonus Buy tiers from the game design:

- **80x bet** -- Triggers Party Mode (equivalent to 3 scatters, 8 free spins)
- **200x bet** -- Triggers Darty Mode (equivalent to 4 scatters, 12 spins, 5x5 grid)
- **500x bet** -- Triggers Full Send Mode (equivalent to 5 scatters, 12 spins, 5x6 grid, 3x starting multiplier)
- **69x bet** -- Random bonus mode (could be any of the above)

If a player's current bet is $1.00 (100 cents), the 80x Bonus Buy costs $80.00 (8000 cents). The 500x costs $500.00 (50000 cents). These are significant amounts. The UI must make the cost absolutely clear before the player confirms.

### The Payment Flow

A Bonus Buy is just a large bet. It follows the same atomic transaction pattern:

1. Player selects a Bonus Buy tier
2. Display the total cost clearly: "Buy Party Mode for $80.00?"
3. Player confirms
4. Debit the Bonus Buy cost from their balance (same as a regular bet)
5. Start the bonus round immediately

There's no Stripe involved in the Bonus Buy itself -- it deducts from the existing wallet balance. The player already deposited funds. Bonus Buy just spends them differently.

### Why This Is Feature Gating

The concept maps directly to SaaS feature gating:
- **SaaS:** "Pay $X/month to access this feature"
- **Game:** "Pay 80x your bet to access this bonus round now"

Both gate access to premium experiences behind payment. The implementation pattern is the same: check if the player can afford it, process the payment, unlock the feature.

### When Directing Claude Code

> "Claude, implement Bonus Buy for Frat House Frenzy. Four tiers: 80x (Party Mode), 200x (Darty Mode), 500x (Full Send Mode), 69x (Random). Display the cost in dollars based on current bet size. Require explicit confirmation before purchase. Process as a balance deduction using the same atomic transaction pattern as regular spins. The Bonus Buy directly starts the corresponding bonus round."

---

## Part 7: Transaction History and Audit Trails

Every cent that moves through Frat House Frenzy must be recorded. Not just for debugging -- for trust, regulatory compliance, and dispute resolution.

### What Gets Recorded

Every balance-changing event creates a transaction record:

- **Deposits:** Amount, Stripe payment ID, timestamp
- **Bets:** Amount, spin ID, timestamp
- **Wins:** Amount, spin ID, timestamp
- **Bonus Buys:** Amount, tier, spin ID, timestamp
- **Withdrawals:** Amount, status (pending/approved/rejected), admin who processed it, timestamp
- **Refunds:** Amount, reason, Stripe refund ID, timestamp
- **Adjustments:** Manual admin adjustments with reason (for customer support cases)

Each record includes the player's balance before and after the transaction. This creates an unbroken chain: you can reconstruct any player's balance at any point in time by replaying their transaction history.

### The Audit Trail

The transaction history is your audit trail. If a player says "I deposited $50 but only got $25," you can show every transaction. If a regulator asks how a player's balance changed over a period, you can produce the exact sequence.

This means transaction records are append-only. You never update or delete a transaction record. If something needs to be corrected, you create a new adjustment record that references the original. The history is immutable.

### When Directing Claude Code

> "Claude, create a transactions table that records every balance change. Fields: player ID, type (deposit, bet, win, bonus_buy, withdrawal, refund, adjustment), amount in integer cents, balance before, balance after, reference ID (Stripe payment ID or spin ID), description, and timestamp. Records are append-only -- never update or delete. Add an index on player ID and timestamp for fast history queries. Build a player-facing transaction history page showing deposits, withdrawals, and a running balance."

---

## Part 8: Why Financial Accuracy Matters More in a Game

In a SaaS, a billing error is inconvenient. A customer gets charged twice, you refund them, you apologize, life goes on. The stakes are low because the amounts are predictable and the transactions are infrequent.

In a game with real money, financial accuracy is existential.

### The Trust Problem

Players are trusting you with their money in a game of chance. That requires more trust than a SaaS subscription because the outcomes are unpredictable. If a player suspects the game is unfair -- even if it's just a display bug showing the wrong balance -- they'll leave and tell everyone.

This is why Frat House Frenzy uses provably-fair mechanics. Every spin result can be independently verified. But provably-fair only proves the game is fair. You also need to prove the money is handled correctly.

### The Regulatory Dimension

Online gaming is regulated in most jurisdictions. Regulations typically require:
- Complete transaction records (your audit trail)
- Accurate RTP (Return to Player) tracking
- Responsible gaming features (session limits, deposit limits, self-exclusion)
- The ability to freeze and investigate accounts

You're building a learning project, not a licensed casino. But building with these principles teaches you how real financial systems work. The patterns -- atomic transactions, immutable audit logs, reconciliation -- apply to any application that handles money.

### The Reconciliation Discipline

Reconciliation means verifying that your internal records match external reality. For Frat House Frenzy:

- Does the sum of all deposits (from Stripe records) match the sum of all deposit transactions in your database?
- Does every player's current balance equal their initial deposits plus wins minus bets minus withdrawals?
- Does the total money in all player wallets match the total deposits minus total withdrawals?

If any of these checks fail, something is wrong. Building reconciliation checks early catches bugs before they become financial disasters.

### When Directing Claude Code

> "Claude, add a reconciliation check that verifies: (1) every player's stored balance equals their computed balance from transaction history, (2) total system balance equals total deposits minus total withdrawals. This should be an admin-only endpoint that can run on demand. Log any discrepancies with full details."

---

## Part 9: Testing Payments

You will never use real money during development. Stripe provides a complete test environment, and your game balance system can be tested with seed data.

### Test Mode

Stripe has two modes: test and live. In test mode, no real money moves. All API calls use test keys (they start with `sk_test_` and `pk_test_`). Test mode has its own dashboard, its own data, and its own webhook events.

### Test Card Numbers

Stripe provides specific card numbers for testing:
- `4242 4242 4242 4242` -- Always succeeds
- `4000 0000 0000 0002` -- Always declines
- `4000 0000 0000 3220` -- Requires 3D Secure authentication

Use these to test the full deposit flow: create a checkout session, complete payment with a test card, receive the webhook, and verify the wallet balance updates.

### Stripe CLI for Webhooks

In production, Stripe sends webhooks to your live URL. In development, your app is running on localhost -- Stripe can't reach it. The Stripe CLI solves this by forwarding webhooks to your local machine.

Run `stripe listen --forward-to localhost:3000/api/webhooks/stripe` and the CLI creates a tunnel. When you complete a test payment, the CLI forwards the webhook to your local app.

### Testing Balance Operations

Beyond Stripe, test your balance management thoroughly:
- Spin when balance is exactly the bet amount (should succeed, balance becomes 0)
- Spin when balance is one cent less than the bet (should reject)
- Two simultaneous spin requests (only one should succeed)
- Deposit while a spin is in progress (should not interfere)
- Withdrawal request for more than the playable balance (should reject)

### When Directing Claude Code

> "Claude, set up Stripe test mode for deposits. Add test instructions to the README for running the Stripe CLI with `stripe listen`. Create seed data that gives test players a starting balance of $100 (10000 cents) for development. Write a test helper that simulates deposit webhooks for automated testing."

---

## What's Next

Head to [project.md](project.md) to add the wallet and payment system to Frat House Frenzy.
