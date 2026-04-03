# Project: Speed Build -- 5 Days, Start to Ship

**What you're building:** A complete application, chosen from the ideas below, built and shipped in 5 days.

**Time estimate:** 15-20 hours across the week (3-4 hours per day)

**What you'll need open:** Ghostty, your browser, a timer (seriously -- time-box yourself)

---

## The Brief

Pick a project. Research it. Plan it. Build it. Test it. Ship it. Five days.

This is not about perfection. It's about proving you can move at professional speed while maintaining quality.

---

## Project Ideas

Pick something different from your Week 15 project. These are scoped for speed -- each can be built in 5 days with the stack you know.

1. **Landing Page Generator** -- Users create a product landing page by filling in a form (headline, features, CTA, images). Generates a live page with a unique URL.
2. **Invoicing Tool** -- Create invoices with line items, tax calculation, and PDF export. Send invoice links to clients.
3. **URL Shortener with Analytics** -- Shorten URLs and track clicks with geographic and referrer data. Dashboard shows link performance.
4. **Poll/Survey Tool** -- Create polls or surveys, share a link, collect responses, see real-time results.
5. **Link-in-Bio Builder** -- Users create a customizable page of links (like Linktree). Track click analytics.
6. **Event RSVP App** -- Create events with details, share invite links, collect RSVPs, view attendee list.
7. **Waitlist Page Builder** -- Create a landing page with email capture, referral tracking, and a dashboard showing signups.
8. **Bookmark Manager** -- Save, tag, and organize bookmarks. Import from browser. Search and filter.
9. **Markdown Note App** -- Write and organize notes in markdown. Folder structure. Full-text search.
10. **Status Page** -- Create a status page for your projects. Manual incident updates. Uptime history.

Pick one. Commit to it. Start Day 1.

---

## Day 1: Research + Plan

**Goal:** End the day with a working project skeleton -- auth, database, and navigation. No features yet.

### Morning: Research (1-2 hours)

**Choose your project (15 minutes max):**
Pick from the list or use your own idea. Don't deliberate. Pick and go.

**Quick competitive scan (30 minutes):**
> "What are the top 3 existing tools for [your project idea]? Give me a one-line description of each and one thing each does well."

You don't need a deep analysis. You just need to know what exists so you don't miss something obvious.

**Define your must-have features (30 minutes):**
Write a list of 3-5 features. That's it. No nice-to-haves this week.

Example for a URL shortener:
1. Create short URLs from long URLs
2. Track clicks on each short URL
3. Dashboard showing all URLs and their click counts
4. User authentication (each user sees their own URLs)

### Afternoon: Plan + Scaffold (2-3 hours)

**Architecture planning:**
> "I'm building a [project name] -- [one-sentence description]. Must-have features: [list]. Use Next.js 15, TypeScript, Tailwind, shadcn/ui, Postgres with Drizzle, and Better Auth. Design the database schema and page structure. Keep it as simple as possible."

**Project setup:**
> "Create a new Next.js project called [name] with TypeScript, Tailwind, and the App Router."

> "Set up Postgres with Drizzle. Create the database schema we planned."

> "Set up Better Auth with email/password authentication."

> "Create the basic layout with navigation."

**Deploy the skeleton:**
> "Set up this project for Vercel deployment. Create the GitHub repo, push, and connect to Vercel."

**Day 1 Deliverable:** A deployed project skeleton with auth, database, and navigation working. Zero features, but the foundation is solid.

---

## Day 2: Build Core Features (Part 1)

**Goal:** Build the first 2-3 must-have features.

### Build Feature 1 (2-3 hours)

Take your most important feature -- the one thing your app absolutely must do.

> "Build the [feature name]. Here's how it works: [user flow description]. Store data in the [tables] we created. Use shadcn/ui components for the interface."

Test it in the browser. Does it work? Does data persist? Commit it.

### Build Feature 2 (1-2 hours)

Move to the next feature. Same pattern: describe, build, verify, commit.

### Deploy and Verify

Push to main. Check the live site. Make sure everything works on Vercel, not just localhost.

**Day 2 Deliverable:** The core action of your app works. A user can sign up, do the main thing, and see the result. Deployed and live.

---

## Day 3: Build Core Features (Part 2)

**Goal:** Finish remaining must-have features. Feature-complete by end of day.

### Build Remaining Features (3-4 hours)

Build your remaining must-have features one at a time. Remember the rules:
- One feature at a time
- Test before moving on
- Commit after each feature
- If it's taking more than 3 hours, simplify

### Polish the Core Flow (30 minutes)

Walk through the entire app as a new user. Is the core flow smooth? Are there any confusing moments?

> "Review the core user flow from sign-up to [main action]. Fix any rough spots: confusing UI, missing navigation, unclear labels."

### Deploy and Verify

Push everything. Check the live site.

**Day 3 Deliverable:** Feature-complete. All must-have features work. The app does what it's supposed to do.

---

## Day 4: Test + Monitor

**Goal:** Add testing and monitoring. Fix bugs found during testing.

### Testing (2-3 hours)

> "Add Vitest to this project. Write unit tests for the core business logic: [describe the key logic to test]. Cover the happy path and the most important edge cases."

> "Set up Playwright and write one E2E test for the critical user flow: sign up, [core action], and verify the result."

Run all tests. Fix any failures.

### Monitoring (1 hour)

> "Set up Sentry error monitoring for this Next.js project."

> "Set up PostHog analytics. Track page views and these key events: [list your core user actions]."

### Bug Fixes (remaining time)

Run through the app manually. Find and fix any bugs. Prioritize: anything that blocks the core flow gets fixed. Cosmetic issues can wait until tomorrow.

**Day 4 Deliverable:** Tests passing. Monitoring active. Core flow works reliably.

---

## Day 5: Polish + Ship

**Goal:** Make it presentable and ship it.

### Performance (1 hour)

Run Lighthouse on the main pages:

> "My Lighthouse scores are: Performance [X], Accessibility [X], Best Practices [X], SEO [X]. Fix the biggest issues to get all scores above 80."

80 is the target this week, not 90. Speed build means good enough, not perfect.

### UI Polish (1 hour)

> "Add loading states, error states, and empty states to all data-fetching pages. Use skeleton loaders for loading and friendly messages for empty states."

> "Do a quick mobile responsiveness check and fix any layout issues on small screens."

### README (30 minutes)

> "Write a README for this project. Include: project description, live demo link, features list, tech stack, and local setup instructions. Keep it concise."

### Screenshots (15 minutes)

Take 3-4 screenshots showing the app in action. Add them to the README.

### Final Deployment (15 minutes)

Push everything. Verify the live site one last time.

- [ ] All features work on the live site
- [ ] Auth works (sign up, sign in, sign out)
- [ ] Data persists across sessions
- [ ] No visible errors in the console
- [ ] README is accurate

**Day 5 Deliverable:** A complete, deployed, documented application. Done.

---

## Acceptance Criteria

- [ ] **New project** (different from Week 15)
- [ ] **Deployed to Vercel** with a live URL
- [ ] **Authentication** working
- [ ] **Core features** working (3-5 features)
- [ ] **Database** with persistent data
- [ ] **Unit tests** covering core logic
- [ ] **E2E test** covering the critical flow
- [ ] **Sentry** error monitoring active
- [ ] **PostHog** analytics tracking key events
- [ ] **README** with description, demo link, and screenshots
- [ ] **Completed in 5 days**

---

## Reflection

You just built a production application in 5 days. Take a moment to recognize what that means.

16 weeks ago, you had never used a terminal. Now you can research, plan, build, test, monitor, and ship a complete application in a week. That's not an incremental improvement. That's a transformation.

And here's the thing: this gets easier every time. The next project will be faster. And the one after that. The workflow is yours now.
