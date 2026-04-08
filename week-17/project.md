# Project: Frat House Frenzy — Advanced Patterns

**What you're building:** Extracting the game engine into a publishable npm package, adding real-time multiplayer features, and optimizing performance for production scale.

**Time estimate:** 12-15 hours across the week

**What you'll need open:** Ghostty, your browser, Chrome DevTools Performance tab, npm

---

## The Brief

The game is feature-complete. Now you apply advanced engineering patterns to make it production-grade and reusable. You'll extract the core engine into a standalone package that other developers could use to build their own games. You'll add real-time multiplayer features. You'll optimize rendering performance so the game runs smoothly even on lower-end devices. These are the patterns that separate a project from a product.

---

## Day 1: Extract the Game Engine Package

### Step 1: Identify the Package Boundary

The game engine -- RNG, symbol mapping, win evaluation, feature triggers, math model -- is domain logic that doesn't depend on React, Next.js, or any UI framework. It should be extractable into a standalone package.

> "Review the Frat House Frenzy codebase and identify every piece of code that is pure game engine logic -- no React, no Next.js, no DOM, no UI. This includes: the provably fair RNG, symbol generation and mapping, reel strip definitions, win evaluation (ways-to-win calculation), all feature mechanics (xNudge, Beer Pong Respin, Party Foul, xWays, bonus modes, Blackout Meter, Tip Jar, Full Send Jackpot), the math model, and the game state machine. List every file and function that should be extracted, and identify any UI dependencies that need to be removed."

### Step 2: Create the npm Package

> "Create a new npm package called 'frat-house-engine' (or a scoped name like @[username]/slot-engine) in a separate directory. Set up: TypeScript with strict mode, dual ESM/CJS output, Vitest for testing, proper package.json with name, version, description, main, module, types, exports, keywords, and license fields. The package should export a clean public API: createGame(), spin(), evaluateWins(), verifyFairness(), and configuration types for symbol definitions and math model parameters."

### Step 3: Extract and Generalize

> "Extract the game engine code into the new package. Remove all app-specific references -- no React imports, no database calls, no API routes. The engine should be a pure function library: give it a configuration (symbols, reel strips, RTP target, feature probabilities) and it returns game results. Make the symbol set configurable -- someone should be able to define their own symbols and pay tables without modifying engine code. Keep the provably fair system intact. Run all existing engine unit tests against the extracted package to verify nothing broke."

### Step 4: Write Package Documentation

> "Write a comprehensive README for the engine package. Include: what it does (a provably fair slot game engine), installation instructions, quick start example showing a basic spin with win evaluation, API reference for all exported functions and types, configuration guide showing how to define custom symbols and pay tables, the provably fair verification flow, and a section on the math model explaining RTP and volatility configuration. Include TypeScript examples for all code samples."

### Step 5: Publish and Verify

> "Prepare the package for npm publishing. Verify the build output includes both ESM and CJS bundles plus type definitions. Create a test script that installs the published package in a fresh project and runs a basic spin to verify it works. Publish to npm."

After publishing, update Frat House Frenzy to import the engine from the npm package instead of local files:

> "Update Frat House Frenzy to use the published engine package as a dependency instead of the local engine code. Remove the duplicated engine files from the game project. Run all tests to verify the game works identically with the packaged engine."

---

## Day 2: WebSocket Multiplayer Lobby

### Step 6: Set Up WebSocket Server

> "Add WebSocket support to the Frat House Frenzy Next.js app. Set up a WebSocket server (using the ws library or Socket.io) that handles player connections. Create a lobby system where connected players can see: who's currently online, a live feed of big wins (20x+) from other players, and a leaderboard of the top 10 biggest wins in the last 24 hours. Use proper connection lifecycle management -- handle disconnects, reconnects, and cleanup."

### Step 7: Build the Live Feed

> "Build a real-time win feed component that shows big wins from all connected players as they happen. When any player hits a win of 20x or higher, broadcast it to all connected clients. Display: player name (or anonymous ID), win amount as a multiplier, which feature triggered it (xNudge, Beer Pong, bonus round, etc.), and a timestamp. The feed should show the last 20 wins and auto-scroll. Animate new entries sliding in from the top. Add a 'Big Win' sound effect when someone else hits a major win."

### Step 8: Build the Leaderboard

> "Build a real-time leaderboard component. Track the biggest wins in the last 24 hours. Display: rank, player name, win multiplier, and time. Update in real-time via WebSocket when a new win enters the top 10. Add a subtle highlight animation when rankings change. Store leaderboard data in PostgreSQL with a scheduled cleanup of entries older than 24 hours."

### Step 9: Lobby UI

> "Create a lobby sidebar or overlay that shows: current online player count (updated in real-time), the live win feed, and the leaderboard. The lobby should be collapsible so it doesn't interfere with gameplay. On mobile, make it a slide-out panel. Add a toggle to mute the lobby notifications."

---

## Day 3: Server-Side Rendering and API Hardening

### Step 10: SSR Optimization

> "Optimize the Frat House Frenzy Next.js app for server-side rendering. The game shell (layout, navigation, balance display, lobby) should render on the server for fast initial load. The game grid and animations should be client-only components (use 'use client' appropriately). Implement proper code splitting so the heavy game engine and animation libraries only load when the player navigates to the game page. Measure the improvement: what's the Time to First Byte and Largest Contentful Paint before and after?"

### Step 11: API Rate Limiting

> "Add rate limiting to all game API routes. Implement: per-user spin rate limiting (max 2 spins per second to prevent automation), per-IP connection limiting for WebSocket (max 5 connections per IP), and per-user Bonus Buy rate limiting (max 1 purchase per 5 seconds). Use a sliding window algorithm stored in Redis (or in-memory if Redis isn't available). Return proper 429 status codes with Retry-After headers. Write tests that verify rate limits are enforced and that normal play isn't affected."

### Step 12: Abuse Prevention

> "Add abuse prevention measures. Implement: server-side validation of all spin requests (verify bet amount is within range, verify player has sufficient balance, verify game state is consistent), nonce tracking to prevent replay attacks on the provably fair system (each nonce must be strictly incrementing, reject any nonce that's been used), and suspicious activity logging (flag accounts that spin faster than humanly possible, always bet max, or show patterns consistent with automation). Log flagged activity to a dedicated database table and the admin dashboard."

---

## Day 4: Provably Fair Verification Page

### Step 13: Build the Verification Page

> "Build a standalone provably fair verification page as its own route (/verify). The page should let anyone verify any past spin without being logged in. Input fields: server seed (revealed after rotation), client seed, nonce. The page should: compute HMAC-SHA256(serverSeed, clientSeed:nonce), map the hash to symbol positions using the same algorithm the engine uses, display the resulting reel layout, and show whether the displayed result matches. Include a step-by-step explanation of the algorithm alongside the verification tool. Add a 'Verify My Last 10 Spins' button for logged-in users that auto-fills their recent game data."

### Step 14: Make It a Reusable Component

> "Extract the provably fair verification UI into a self-contained React component that could be dropped into any game. The component should accept configuration props: hash algorithm name, seed format description, and a mapping function that converts a hash to game results. Package it so it could be published separately or imported by any project using the game engine. Write Storybook stories (or a simple demo page) showing the component configured for Frat House Frenzy and for a hypothetical different game."

---

## Day 5: Performance Optimization

### Step 15: Profile the Game

> "Profile the game's rendering performance using Chrome DevTools. Play through several complex scenarios: a cascade chain of 5+, a bonus round with Blackout activation, xWays expansion with multiple funnels, and the Full Send Jackpot. Capture a Performance recording for each. Identify: any frames that drop below 60fps, which components cause the most re-renders, memory usage during extended play sessions, and whether animations cause layout thrashing."

### Step 16: Canvas vs DOM Rendering Analysis

> "Analyze whether the game grid should use canvas rendering instead of DOM elements for the symbol animations. Build a small proof-of-concept: render the 5x4 grid with cascading symbol animations using HTML Canvas (or a library like PixiJS). Compare performance metrics against the current Framer Motion DOM approach for: a 5-cascade chain, a full 5x7 expanded grid, and 10+ simultaneous animated elements. Document the trade-offs: canvas gives better animation performance but loses React component composition and accessibility. Recommend which approach to use for production and implement the recommendation."

### Step 17: Optimize for Production

> "Apply production performance optimizations. Implement: symbol sprite sheets (one image load instead of individual symbol images), animation object pooling (reuse particle effect objects instead of creating/destroying them), lazy loading of bonus round assets (don't load Blackout or Full Send assets until the first bonus triggers), Web Worker for heavy math (run RTP simulations and win calculations off the main thread), and requestAnimationFrame-based animation timing (replace any setTimeout/setInterval in animation code). Measure the performance improvement for each optimization."

---

## Acceptance Criteria

- [ ] **Game engine published to npm** as a standalone package with documentation
- [ ] **Frat House Frenzy uses the published package** -- no duplicated engine code
- [ ] **WebSocket lobby** with live player count, big win feed, and 24-hour leaderboard
- [ ] **SSR optimized** -- game shell renders server-side, game grid is client-only, proper code splitting
- [ ] **API rate limiting** on spin, WebSocket, and Bonus Buy endpoints
- [ ] **Abuse prevention** -- nonce tracking, server-side validation, suspicious activity logging
- [ ] **Provably fair verification page** -- standalone route with step-by-step algorithm explanation
- [ ] **Verification component** extracted as a reusable, configurable React component
- [ ] **Performance profiled** with documented findings for complex game scenarios
- [ ] **Canvas vs DOM analysis** completed with recommendation implemented
- [ ] **Production optimizations** applied -- sprite sheets, object pooling, lazy loading, Web Worker
- [ ] **60fps maintained** during the most complex game states on mid-range hardware
- [ ] **All existing tests pass** after all changes
- [ ] **Deployed to Vercel** with all features working

---

## Reflection

This week you applied patterns that most professional developers learn over years of experience. Extracting a library from a working application. Adding real-time features. Hardening APIs against abuse. Profiling and optimizing rendering performance. These aren't beginner skills -- they're the skills that make the difference between a developer who can build things and a developer who can build things that scale.

The game engine package you published is a real open-source contribution. Someone else could use it to build their own game. That's the pattern: build something specific, extract the general, share it with the world.
