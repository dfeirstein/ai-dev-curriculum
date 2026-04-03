# Project: Authenticated Bookmark Manager with Monitoring

**What you're building:** Taking your Week 5 bookmark manager and adding user accounts, error monitoring, product analytics, and welcome emails. Each user gets their own private bookmarks. Production-grade observability.

**Time estimate:** 8-10 hours

**What you'll need open:** Ghostty (your terminal), a web browser, and accounts on Sentry, PostHog, and Resend

---

## The Brief

Your bookmark manager works, but everyone shares the same bookmarks. That's not an application -- that's a shared notepad. This week you're adding the business layer: user accounts so each person has their own bookmarks, monitoring so you know when things break, analytics so you know how the app is being used, and email so users get a welcome message when they sign up.

By the end, you'll have something that looks and functions like a real SaaS product.

---

## Phase 1: Research

Start Claude Code and research the integration approach:

> "Claude, I have an existing Next.js bookmark manager with Drizzle ORM and Postgres on Neon. I want to add Better Auth for user accounts (email/password and GitHub OAuth), Sentry for error monitoring, PostHog for analytics, and Resend for transactional email. What's the best order to integrate these? What changes need to happen to my existing database schema?"

Follow up:

> "How does Better Auth work with Drizzle ORM? Does it create its own tables?"

> "What do I need to change so bookmarks are scoped to individual users?"

> "What events should I track with PostHog for a bookmark manager?"

---

## Phase 2: Plan

> "Claude, plan the integration of Better Auth, Sentry, PostHog, and Resend into my existing bookmark manager. The key changes are: add user accounts, make bookmarks user-scoped (each user only sees their own), add protected routes, add error monitoring, add analytics events, and send a welcome email on signup. Plan the database changes, the auth flow, and each integration step by step."

Review the plan. Key questions to ask yourself:

- What happens to the existing bookmarks when I add user scoping?
- Which routes need to be protected?
- What events am I tracking and why?
- Where do the API keys for each service go?

---

## Phase 3: Execute

### Step 1: Set Up Service Accounts

Before integrating, create your free accounts:

1. **Sentry** -- Go to [sentry.io](https://sentry.io), create a free account, and create a new project (select Next.js as the platform). Copy the DSN (a URL that identifies your project).
2. **PostHog** -- Go to [posthog.com](https://posthog.com), create a free account. Copy your project API key.
3. **Resend** -- Go to [resend.com](https://resend.com), create a free account. Create an API key.

For **GitHub OAuth** (used by Better Auth):
1. Go to GitHub > Settings > Developer Settings > OAuth Apps > New OAuth App
2. Set the callback URL to `http://localhost:3000/api/auth/callback/github` for development

Tell Claude Code about your credentials:

> "Claude, add these environment variables to .env.local: SENTRY_DSN, NEXT_PUBLIC_POSTHOG_KEY, RESEND_API_KEY, GITHUB_CLIENT_ID, GITHUB_CLIENT_SECRET. I'll fill in the values."

Add your actual keys to the `.env.local` file.

### Step 2: Integrate Better Auth

> "Claude, integrate Better Auth into the bookmark manager. Set it up with email/password signup and GitHub OAuth. Use our existing Drizzle ORM and Postgres database. Create the auth tables with a migration. Add login and signup pages using shadcn/ui components."

Test it:
- Can you sign up with email and password?
- Can you log in?
- Can you log out?

If GitHub OAuth is configured:
- Can you log in with GitHub?

### Step 3: Make Bookmarks User-Scoped

> "Claude, update the bookmarks schema to include a userId column that references the auth users table. Run the migration. Update all API routes so users can only see, create, edit, and delete their own bookmarks. Any request for another user's bookmark should return 404."

This is the critical change. Test thoroughly:
- Sign up as User A, create some bookmarks
- Log out, sign up as User B
- Verify User B sees no bookmarks (their own empty state)
- User B creates bookmarks
- Log back in as User A -- only User A's bookmarks should appear

### Step 4: Add Protected Routes

> "Claude, protect the /bookmarks routes. If a user is not logged in, redirect them to the login page. Add a navigation bar that shows the user's email and a logout button when logged in, and login/signup buttons when logged out."

Test:
- Log out
- Try to visit /bookmarks directly
- You should be redirected to the login page
- Log in and you should be sent to /bookmarks

### Step 5: Add Sentry Error Monitoring

> "Claude, add Sentry for error monitoring. Configure it for both client and server components. Set up source maps. Add a test button (only visible in development) that throws an error so I can verify Sentry catches it."

Test:
- Click the test error button
- Verify the error appears in Sentry. You can check manually, or use the Chrome tool:

> "Claude, use the Chrome tool to open my Sentry dashboard and verify that the test error appears in the Issues tab with full context."

Remove the test button after verifying:

> "Claude, remove the test error button now that Sentry is verified."

### Step 6: Add PostHog Analytics

> "Claude, add PostHog analytics. Track these events: user-signed-up (on signup), user-logged-in (on login), bookmark-created (with tag count), bookmark-deleted, bookmark-searched (with search query), bookmark-filtered-by-tag (with tag name). Initialize PostHog in the app layout."

Test:
- Perform each action (sign up, log in, create bookmark, search, etc.)
- Verify events in PostHog. Use the Chrome tool:

> "Claude, use the Chrome tool to open my PostHog dashboard, go to Events, and verify that the signup and bookmark-created events appear."

### Step 7: Add Welcome Email

> "Claude, set up Resend for transactional email. When a new user signs up, send them a welcome email with their name, a brief message welcoming them to the bookmark manager, and a link to get started. Use a clean, simple template."

Test:
- Sign up with a new email address
- Check your inbox for the welcome email
- Verify the content looks good

Note: On the free tier, Resend can only send to the email address you signed up with. That's fine for testing.

### Step 8: Polish

> "Claude, add a landing page at / that explains what the bookmark manager does, with login and signup buttons. Only show this to logged-out users -- logged-in users should be redirected to /bookmarks."

> "Claude, make sure all error states are handled gracefully. If the database is unreachable, if an API call fails, if auth fails -- the user should see a friendly message, not a crash. Sentry should capture the error."

### Step 9: Quality Gates

Run the full pipeline:

> "Claude, run npm run lint and fix any issues."

> "Claude, run npm run typecheck and fix any issues."

> "Claude, run npm run build and make sure it completes without errors."

### Step 10: Deploy

> "Claude, commit all changes, push to GitHub, and deploy to Vercel. I need to add the new environment variables (SENTRY_DSN, NEXT_PUBLIC_POSTHOG_KEY, RESEND_API_KEY, GITHUB_CLIENT_ID, GITHUB_CLIENT_SECRET, BETTER_AUTH_SECRET) to the Vercel dashboard. Walk me through it."

After deploying, update the GitHub OAuth callback URL to your production URL:

> "Claude, what do I need to change in the GitHub OAuth app settings for production?"

Test everything on the live URL:
- Sign up
- Check for welcome email
- Create bookmarks
- Verify they're private (different user sees different bookmarks)
- Trigger an error -- does Sentry catch it?
- Check PostHog -- are events flowing?

---

## Acceptance Criteria

Your project is complete when all of these are true:

- [ ] App is live on Vercel with user authentication
- [ ] Users can sign up with email/password
- [ ] Users can log in with GitHub OAuth
- [ ] Each user only sees their own bookmarks
- [ ] Unauthenticated users are redirected to login
- [ ] Navigation shows user info when logged in
- [ ] Sentry is capturing errors (verified in dashboard)
- [ ] PostHog is tracking events (verified in dashboard)
- [ ] Welcome email is sent on signup (verified in inbox)
- [ ] Landing page explains the app for logged-out users
- [ ] All API keys are in environment variables (never in code)
- [ ] `npm run lint` passes
- [ ] `npm run typecheck` passes
- [ ] `npm run build` completes without errors
- [ ] Code is on GitHub

---

## Stretch Goals

**Add Google OAuth**
> "Claude, add Google OAuth as an additional login option alongside GitHub. Walk me through setting up the Google OAuth credentials."

**Add a User Settings Page**
> "Claude, add a /settings page where users can update their display name, change their password, and delete their account. Use shadcn/ui form components."

**Add Stripe with Free/Pro Tiers**
> "Claude, integrate Stripe with two tiers: Free (50 bookmark limit) and Pro ($10/month, unlimited bookmarks). Add a Checkout flow for upgrading to Pro. Set up webhooks for payment events. Show the current plan in the user settings. Add a feature gate that prevents Free users from creating more than 50 bookmarks."

**Add a Public Profile**
> "Claude, add a public profile page at /u/[username] that shows a user's public bookmarks. Add a toggle on each bookmark to mark it as public or private."

---

## Troubleshooting

**Better Auth login/signup isn't working**
> "Claude, the auth flow is broken. Here's what happens: [describe what you see]. Check the Better Auth configuration, the database tables, and the auth API routes."

**OAuth redirect fails**
> "Claude, after clicking 'Login with GitHub' I get redirected to an error page. Check the OAuth callback URL, the client ID and secret, and the Better Auth OAuth configuration."

**Sentry isn't capturing errors**
> "Claude, I triggered a test error but nothing shows up in Sentry. Check the SENTRY_DSN environment variable, the Sentry initialization, and whether source maps are configured correctly."

**PostHog events aren't appearing**
> "Claude, I performed actions in the app but PostHog dashboard shows no events. Check the PostHog API key, the initialization code, and whether events are being sent from the correct components."

**Welcome email not arriving**
> "Claude, I signed up but no welcome email arrived. Check the Resend API key, the email sending code, and whether the from address is verified in Resend."

**Bookmarks from before auth still visible**
> "Claude, old bookmarks from before I added auth don't have a userId. They're showing up for everyone or for nobody. Write a migration or cleanup script to handle orphaned bookmarks."

---

## Reflection

Look at what you've built. A web application with:
- User accounts with multiple login methods
- Private, per-user data storage
- Error monitoring that catches production bugs
- Analytics that track user behavior
- Automated transactional email
- A polished frontend with professional components
- A real database with validated data
- Deployed and live on the internet

Six weeks ago, you'd never built software. Now you've directed the construction of a production-grade application using the same tools and architecture that real SaaS companies use.

You didn't memorize programming syntax. You didn't debug semicolons. You learned what each piece of the stack does, why it exists, and how to tell Claude Code to assemble it correctly. That's the AI-native builder skill set, and you now have a solid foundation in it.
