# Quality Gates Cheatsheet

> Commands to run after Claude Code makes changes. Your job isn't to fix these — it's to notice them and tell Claude Code to fix them.

---

## The Pipeline

Run these in order after every significant change:

```
1. npm run lint          → Code quality check
2. npm run typecheck     → Type safety check
3. npm run test          → Automated tests pass
4. npm run build         → Production build succeeds
5. Open in browser       → Visual review
```

If any step fails: copy the error, paste it to Claude Code, and say "Fix this."

---

## What Each Gate Catches

### `npm run lint` (ESLint)
**What it catches:** Bad patterns, unused variables, accessibility issues, potential bugs.
**Why it matters:** Prevents sloppy code from accumulating.
**If it fails:** "Claude, fix the lint errors."

### `npm run typecheck` (TypeScript)
**What it catches:** Type mismatches, missing properties, impossible states.
**Why it matters:** If it passes, the code structure is valid. TypeScript is your most reliable automated reviewer.
**If it fails:** "Claude, fix the type errors."

### `npm run test` (Vitest)
**What it catches:** Broken functionality — features that used to work but don't anymore.
**Why it matters:** Prevents regressions. New changes shouldn't break old features.
**If it fails:** "Claude, these tests are failing: [paste output]. Fix the code or update the tests if the expected behavior changed."

### `npm run build` (Next.js)
**What it catches:** Build-time errors, missing imports, configuration issues.
**Why it matters:** If it doesn't build, it can't be deployed. This is the final code check.
**If it fails:** "Claude, the build is failing: [paste error]. Fix it."

### Visual Review (Browser)
**What it catches:** UI issues, wrong behavior, missing features.
**Why it matters:** Automated checks can't tell you if the app LOOKS right or BEHAVES as expected.
**How to do it:**
1. Run `npm run dev`
2. Open the URL in your browser
3. Click around — does it match what you asked for?
4. Check mobile view (browser dev tools → device mode)

---

## Automated Quality (Set Up Once)

### Claude Code Hooks
Configure hooks so quality checks run automatically:
- "Claude, add a hook that runs lint after every file edit"
- "Claude, add a hook that runs type-check before every commit"

### GitHub Actions CI
Set up once, runs on every PR:
- Lint → Type-check → Test → Build
- PR can't merge if any step fails

### Vercel Preview Deployments
Every PR gets a live URL. Click it, review the changes in a real environment.

---

## Quick Reference

| Situation | Command | Tell Claude If It Fails |
|-----------|---------|------------------------|
| After any change | `npm run lint` | "Fix the lint errors" |
| After any change | `npm run typecheck` | "Fix the type errors" |
| After feature work | `npm run test` | "These tests are failing: [paste]" |
| Before deploy | `npm run build` | "The build is failing: [paste]" |
| Before deploy | Open in browser | "This doesn't look right: [describe issue]" |
| Performance check | Lighthouse audit | "The Lighthouse score is [X]. Improve it." |

---

## The Golden Rule

**You don't fix errors. You notice them and direct Claude Code to fix them.**

Your job is quality inspector, not mechanic. Run the checks, report the failures, and Claude Code does the rest.
