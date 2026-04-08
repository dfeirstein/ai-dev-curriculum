# Frat House Frenzy — Game Design Brief

> The student's chosen project. This replaces TeamTask Pro as the primary build project (Weeks 8-14) and extends into Weeks 15-18 for advanced features.

---

## Overview

**FRAT HOUSE FRENZY** is a provably fair online slot game. Dark comedy frat party theme with escalating chaos mechanics. Art style: gritty cartoon with neon party lighting. NOT family-friendly.

## Tech Stack Mapping

| Game Component | SaaS Skill Learned | Stack |
|---|---|---|
| Player accounts | Auth (Better Auth) | Next.js + Better Auth |
| Deposits/withdrawals | Payments (Stripe) | Stripe Connect |
| Game engine | Business logic + APIs | Next.js API routes + Zod |
| RNG / Provably fair | Cryptography + validation | Node.js crypto + HMAC-SHA256 |
| Reel UI + animations | Frontend (React + Tailwind) | React + Framer Motion + Tailwind |
| Player balance / game state | Database (PostgreSQL + Drizzle) | PostgreSQL + Drizzle ORM |
| Win notifications | Real-time events | Server-Sent Events or WebSocket |
| Admin dashboard | Dashboard + charts | Recharts + admin routes |
| RTP monitoring | Analytics (PostHog) | PostHog + custom metrics |
| Error tracking | Monitoring (Sentry) | Sentry |
| Sound design | Media integration | Howler.js |
| Automated testing | Testing (Vitest) | Vitest + math model verification |
| Deploy pipeline | CI/CD (GitHub Actions) | GitHub Actions + Vercel |

## Reel Layout

- Base game: 5x4 (1,024 ways) with expanding rows
- During features: up to 5x7 (16,807 ways)
- Cascading wins (tumble mechanic)

## Symbols

**Low pays (solo cups):** Red, Blue, White, Green (0.1x-0.4x)

**High pays (characters):**
- The Pledge (nervous freshman) — 1x-1.5x
- The DJ (headphones, vape cloud) — 1.5x-2x
- The Pong King (backwards cap, gold chain) — 2x-3x
- The House Mom (sunglasses indoors, bathrobe) — 3x-5x
- The Frat President (toga, crown of cans) — 5x-8x

**Special symbols:**
- WILD — The Keg: Standard wild
- xNudge Wild — The Keg Stand: Full-reel wild, +1 multiplier per nudge
- Scatter — The Noise Complaint (police badge): 3/4/5 triggers bonus modes
- Mystery Symbol — Mystery Solo Cup: All reveal same random symbol
- Collector — The Tip Jar: Collects all visible multiplier values

## Math Model

| Metric | Value |
|---|---|
| RTP | 96.09% (default) |
| Volatility | Extreme (10/10) |
| Hit Frequency | 26.3% base game |
| Max Win | 42,069x |
| Bonus Trigger Rate | ~1 in 195 spins |
| Bet Range | $0.10 - $100 |

## Base Game Features

1. **Beer Pong Respin** — Two xNudge wilds lock reels between them, respin, multipliers multiply
2. **Party Foul** (~1 in 40 spins) — Random events: mystery symbols, high-pay-only respin, or random wilds
3. **xWays Funnels** — Mystery positions on reels 2-4 reveal 2-4 matching symbols, expanding ways

## Bonus Rounds

- 3 scatters = Party Mode (8 free spins)
- 4 scatters = Darty Mode (12 spins, 5x5 grid)
- 5 scatters = Full Send Mode (12 spins, 5x6 grid, 3x starting multiplier)
- Bonus Buy: 80x / 200x / 500x / 69x (random)

**During Bonus:**
- Escalation System: Retrigger adds a row + 2 to global multiplier (max 5x7)
- Blackout Meter: Fills with cascades, at 100% triggers 3 sticky-wild spins
- Tip Jar Collector: Absorbs all on-screen multipliers, becomes wild with combined value

## Full Send Jackpot

Triggers when global multiplier reaches 15x+ AND Tip Jar collects 100x+ in one spin:
- 5 guaranteed spins on locked 5x7 grid
- All wilds sticky, multipliers cumulative
- Path to 42,069x max win

## Provably Fair

- Server seed committed (SHA-256 hash pre-spin)
- Client seed editable by player
- HMAC-SHA256(serverSeed, clientSeed:nonce) → symbol mapping
- Full verification page

## Sound Design

- Base: Lo-fi party background, muffled bass
- Cascades: EDM build-up, pitch/tempo rises per chain
- Big win (50x+): Bass drop, crowd roar, strobe
- Bonus entry: Record scratch → silence → "LET'S GO" → beat drop
- Blackout Round: Heartbeat + low drone building
