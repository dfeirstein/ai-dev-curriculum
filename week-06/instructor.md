# Week 6: Auth, Payments, and Third-Party Services — Instructor Guide

## Week at a Glance
- Theme and key concepts: Integrating third-party services into an existing application. Students learn that real apps are assembled from specialized services — auth (Better Auth), monitoring (Sentry), analytics (PostHog), email (Resend) — and how to wire them together without breaking what already works.
- What students ship: The Week 5 bookmark manager upgraded with user accounts, per-user private bookmarks, error monitoring, product analytics, and a welcome email on signup
- Estimated hours: 8-10
- Difficulty: 4

## Pacing Guide
- **Day 1-2:** Read guide.md. Set up accounts on all services (Better Auth config, Sentry, PostHog, Resend). Integrate Better Auth first — this is the biggest change. Add login/signup pages. Scope bookmarks to individual users.
- **Day 3-4:** Add Sentry error monitoring and PostHog analytics. These are lighter integrations but require understanding what to track and why. Add Resend for a welcome email on signup.
- **Day 5:** Test the full flow: sign up (welcome email arrives), log in, create bookmarks (private to user), trigger an error (appears in Sentry), check analytics (events in PostHog). Deploy with all environment variables set.

## Getting Students Started
- Kick off by listing all the services and their one-sentence purpose. Write it on the board: Better Auth = who is this user. Sentry = what broke. PostHog = what are users doing. Resend = talk to users via email. This is the service map for the week.
- Common confusion: students feel overwhelmed by the number of new accounts and dashboards. Reassure them: each integration is independent. If Sentry breaks, PostHog still works. They are adding layers, not rebuilding.
- Students should have open: Ghostty terminal, browser with their app, and tabs for each service dashboard (Sentry, PostHog, Resend). They will reference these dashboards throughout the week.

## Check-in Points
- **After guide.md:** Can the student list all four services and explain what each one does? Can they explain why bookmarks need to change (adding a `userId` column, filtering queries by user)?
- **Mid-project checkpoint:** Better Auth is working — student can sign up, log in, and log out. Bookmarks are scoped to the logged-in user (different users see different bookmarks). This is the critical milestone. If auth is not working by mid-week, the other integrations will not have enough time.
- **Pre-ship review:** Full flow test: create a new account, receive the welcome email (check Resend dashboard if not in inbox), add bookmarks, verify they are private, deliberately trigger an error and find it in Sentry, check PostHog for signup and bookmark events. Deploy with every environment variable set on Vercel.

## Common Pitfalls
- **OAuth configuration confusion.** If using GitHub OAuth with Better Auth, the callback URL must exactly match what is configured in the GitHub OAuth app settings. One wrong character and the entire flow breaks silently. Have students double-check: is the callback URL `http://localhost:3000/api/auth/callback/github` during development and the production URL when deployed?
- **Environment variable chaos.** This week introduces 6-8 new environment variables across four services. Students will inevitably put a Sentry DSN where the PostHog key should go, or forget to add variables to Vercel. Have them maintain a checklist: for each service, what is the variable name, where did they get the value, and is it set both locally and on Vercel?
- **Trying to integrate everything at once.** Student configures all four services simultaneously, nothing works, and they can't isolate which one is broken. Enforce sequential integration: get auth fully working, then add Sentry, then PostHog, then Resend. Commit after each one.
- **Database migration forgotten after adding user scoping.** Adding a `userId` column to the bookmarks table requires a migration. If the student adds the column in the Drizzle schema but forgets to push the migration, they get "column userId does not exist" errors. Same issue as Week 5 — check that the migration ran.
- **Welcome email never arrives.** On Resend's free tier, you can only send to verified email addresses or your own domain. Students will try to send to a random Gmail address and wonder why it never arrives. Have them test with the email address they verified on Resend.

## How to Know the Student Gets It
- They can explain the integration order and why it matters (auth first, because everything else depends on knowing who the user is).
- When adding a new service, they instinctively think about: API key (environment variable), client setup (initialization code), where it gets called (which pages or routes), and how to verify it works (check the dashboard).
- They check dashboards to verify integrations. Not just "the code is there" but "I can see the event in Sentry / PostHog / Resend."
- Red flags: Student's app has login/signup buttons but auth doesn't actually work. Student says "I added Sentry" but has never checked the Sentry dashboard. Environment variables are hardcoded in the source code instead of `.env.local`.

## Support Strategies
- If stuck on auth: Better Auth issues are almost always configuration. Check three things: (1) Is the `AUTH_SECRET` set? (2) Is the database migration run so Better Auth's tables exist? (3) Are the OAuth callback URLs correct? These three cover 90% of auth issues.
- If rushing without reviewing: Ask them to log in as two different users and show you that each sees only their own bookmarks. This single test verifies the most critical feature of the week.
- If frustrated: This is a high-integration week and it is normal to feel like a "configuration monkey." Validate the feeling, then reframe: this is what professional development actually looks like. Most real work is wiring services together, not writing algorithms.

## Differentiation
- **Moving fast?** Add GitHub OAuth in addition to email/password. Add a user profile page with avatar and settings. Add rate limiting on the API routes. Add more granular PostHog events (bookmark created, bookmark deleted, search performed).
- **Struggling?** Focus on Better Auth only. Skip Sentry, PostHog, and Resend. Getting user accounts and private bookmarks working is the essential learning. The monitoring and email integrations can be added later.
- **Minimum viable ship:** User accounts work (sign up, log in, log out). Bookmarks are scoped per user. The app is deployed with auth working in production. At least one additional service (Sentry, PostHog, or Resend) is integrated and verifiably working.
