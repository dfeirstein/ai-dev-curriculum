# Project: TeamTask Pro — Monitoring and Analytics

**What you're building:** Comprehensive monitoring and analytics for TeamTask Pro -- Sentry for error tracking and PostHog for product analytics, feature flags, and session replay.

**Time estimate:** 7-9 hours

**What you'll need open:** Ghostty (your terminal) and a web browser

---

## The Brief

TeamTask Pro is live and tested. Now you need to see what's happening in production. This week you'll add two layers of visibility: Sentry catches errors before users report them, and PostHog shows you how the product is actually being used. By the end, you'll have a real-time view into your application's health and user behavior.

---

## Phase 1: Research

Start Claude Code and research the monitoring approach:

> "Claude, I have a Next.js app (TeamTask Pro) deployed on Vercel. I want to add Sentry for error monitoring with source maps, release tracking, and custom error boundaries. I also want to add PostHog for event tracking, funnels, feature flags, and session replay. What's the best integration approach for a Next.js App Router project?"

Follow up:

> "How do Sentry source maps work with Vercel deployments? Does Vercel handle source map uploading automatically?"

> "How should I structure PostHog event tracking -- should events fire from client components, server components, or API routes?"

> "What's the performance impact of adding Sentry and PostHog? Will it slow down the app noticeably?"

---

## Phase 2: Plan

> "Claude, plan the complete monitoring and analytics integration for TeamTask Pro. I need: (1) Sentry error monitoring with source maps, release tracking, custom error boundaries, and alert rules, (2) PostHog event tracking based on this tracking plan: signup_completed, org_created, project_created, task_created, task_completed, member_invited, dashboard_viewed, csv_exported, subscription_upgraded, subscription_canceled, paywall_shown, (3) PostHog feature flags for a new feature rollout, (4) PostHog session replay for debugging. Plan the integration order and any configuration needed."

Review the plan. Key questions:

- How does Sentry handle errors in both client and server components in Next.js?
- Where does each PostHog event get fired (client vs. server)?
- How do feature flags get evaluated -- on the server or the client?
- What environment variables are needed for both services?

---

## Phase 3: Execute

### Step 1: Sentry Error Monitoring

> "Claude, add Sentry error monitoring to TeamTask Pro. Use the official Sentry Next.js SDK. Configure it for: (1) automatic error capture in both client and server components, (2) source map uploading so production errors show original file names and line numbers, (3) user identification so errors include which user was affected -- use the authenticated user's ID and email, (4) release tracking tied to Git commits so we can correlate errors with deploys. Add the SENTRY_DSN and SENTRY_AUTH_TOKEN to the environment variables."

Test it:

> "Claude, add a temporary test button on the dashboard (only visible in development mode) that throws an intentional error. I'll click it and verify it appears in Sentry."

Click the test button, then check your Sentry dashboard:
- Does the error appear?
- Does it show the correct source file and line number (not minified code)?
- Does it show which user triggered it?
- Does it show breadcrumbs leading up to the error?

> "Claude, remove the test error button now that Sentry is verified."

### Step 2: Custom Error Boundaries

> "Claude, add custom error boundaries to TeamTask Pro that report to Sentry. Create error boundaries for: (1) the root layout -- catch any top-level render error and show a friendly 'Something went wrong' page with a retry button, (2) the dashboard -- if dashboard data fails to load, show an error state without crashing the whole app, (3) individual project pages -- if one project errors, it shouldn't take down the projects list. Each error boundary should report the error to Sentry with context about which boundary caught it."

Test it:

> "Claude, temporarily introduce an error in the dashboard data fetching to verify the error boundary catches it and shows the friendly error state instead of a crash."

Verify, then fix the intentional error.

### Step 3: Sentry Alerts

Configure alerts in the Sentry dashboard (this is done in the Sentry web UI, not in code):

1. Go to your Sentry project > Alerts > Create Alert Rule
2. Create a "First Occurrence" alert: notify you by email when a new error type appears for the first time
3. Create a "High Frequency" alert: notify you when any error occurs more than 10 times in one hour

> "Claude, what alert rules should I set up in Sentry for a production app like TeamTask Pro? Walk me through the configuration in the Sentry dashboard."

### Step 4: PostHog Event Tracking

> "Claude, add PostHog analytics to TeamTask Pro. Initialize the PostHog client in the app layout. Implement the following tracking plan -- fire each event at the appropriate point in the code:

Events to track:
- signup_completed: after successful signup, with property 'method' (email or oauth)
- org_created: after organization creation
- project_created: after project creation, with property 'org_id'
- task_created: after task creation, with properties 'has_due_date', 'has_assignee', 'project_id'
- task_completed: after task completion, with property 'time_to_complete' (days since creation)
- member_invited: after member invitation, with property 'role'
- dashboard_viewed: when the dashboard page loads
- csv_exported: when a CSV export completes, with property 'export_type'
- subscription_upgraded: after successful upgrade, with properties 'from_plan' and 'to_plan'
- paywall_shown: when a free user hits a feature limit

Identify users with their user ID and email after login."

Test it:
- Perform each action in the app (create a task, complete it, view the dashboard, export CSV, etc.)
- Go to your PostHog dashboard
- Verify each event appears with the correct properties

### Step 5: PostHog Feature Flags

> "Claude, set up PostHog feature flags in TeamTask Pro. Create a feature flag called 'new-dashboard-layout' that controls whether users see the existing dashboard or a new version. Implement the flag check in the dashboard page -- when the flag is enabled, show a slightly different dashboard layout (maybe reordered sections or a new summary card). When the flag is disabled, show the current layout. The flag evaluation should happen server-side so there's no flash of the wrong layout."

In the PostHog dashboard:
1. Create the 'new-dashboard-layout' feature flag
2. Set it to roll out to 0% of users initially
3. Enable it for your own user to test
4. Verify you see the new layout while others would see the old one

> "Claude, explain how I would gradually roll this flag out -- first to 10% of users, then 50%, then 100% -- using the PostHog dashboard."

### Step 6: PostHog Session Replay

> "Claude, enable PostHog session replay for TeamTask Pro. Configure it to: (1) record sessions for all users, (2) mask sensitive input fields (passwords, payment info), (3) capture console logs and network requests for debugging context. Set the sample rate to 100% for now (we'll reduce it when we have more users)."

Test it:
- Navigate through the app for a minute -- visit different pages, create something, click around
- Go to PostHog > Session Replay
- Find your session and play it back
- Verify it recorded your navigation accurately
- Verify sensitive fields are masked

### Step 7: Verify the Complete Picture

Now step back and verify that the monitoring and analytics stack is working together:

> "Claude, use the Chrome tool to run through a complete user journey on the dev server: signup, create an org, create a project, create a task, complete the task, view the dashboard, export CSV. After the journey, I'll check both Sentry (no errors should have been captured) and PostHog (all events should appear in order)."

Check Sentry:
- No new errors from the test journey (good -- the happy path works)
- The existing test errors from Step 1 are still there

Check PostHog:
- All events from the journey appear in chronological order
- User identification is correct
- Event properties are populated

### Step 8: Quality Gates and Deploy

> "Claude, run the full quality pipeline: lint, typecheck, test, build. Make sure the Sentry and PostHog integrations don't break any existing tests."

> "Claude, commit all changes with a clear message about monitoring and analytics, push to GitHub, and deploy."

After deployment, verify on the live URL:

> "Claude, add the Sentry and PostHog environment variables to the Vercel dashboard if they aren't there already. Walk me through the configuration."

Visit the production URL and perform a few actions. Check:
- Sentry release tracking: does the new deployment appear as a release?
- PostHog events: are production events flowing?
- Session replay: does a production session get recorded?

---

## Acceptance Criteria

Your project is complete when all of these are true:

- [ ] Sentry is capturing errors with source maps (verified in Sentry dashboard)
- [ ] Sentry errors include user context (user ID and email)
- [ ] Sentry release tracking is connected to deploys
- [ ] Custom error boundaries catch and report errors gracefully
- [ ] Sentry alert rules are configured (first occurrence + high frequency)
- [ ] PostHog event tracking is implemented for all events in the tracking plan
- [ ] PostHog events include correct properties (verified in PostHog dashboard)
- [ ] PostHog user identification works (logged-in users are identified)
- [ ] A feature flag is set up and functional (new-dashboard-layout)
- [ ] PostHog session replay is recording sessions with sensitive data masked
- [ ] No new errors in Sentry from the monitoring/analytics integration itself
- [ ] All existing tests still pass
- [ ] All quality gates pass (lint, typecheck, test, build)
- [ ] Code is on GitHub
- [ ] App is deployed and functional on Vercel with monitoring active

---

## Stretch Goals

**Custom Sentry Dashboard**
> "Claude, help me configure a custom Sentry dashboard that shows: unresolved errors by page, error frequency over the last 7 days, most affected users, and errors by browser/device. Walk me through the Sentry dashboard configuration."

**PostHog Funnel Analysis**
> "Claude, help me set up a funnel in PostHog for the onboarding flow: signup_completed -> org_created -> project_created -> task_created -> task_completed. Walk me through creating this funnel in the PostHog dashboard. What does the data tell us?"

**Analytics Opt-Out**
> "Claude, add an analytics opt-out option to TeamTask Pro. In the user settings, add a toggle that lets users disable PostHog tracking and session replay. When opted out, no events should be sent. Store the preference and respect it across sessions."

**Custom Sentry Performance Monitoring**
> "Claude, enable Sentry performance monitoring for TeamTask Pro. Track transaction durations for key API routes (task creation, dashboard data loading, CSV export). Set up alerts for slow transactions."

---

## Troubleshooting

**Sentry errors show minified code instead of source files**
> "Claude, Sentry is showing minified file names and line numbers. Source maps aren't working. Check the Sentry source map configuration, the SENTRY_AUTH_TOKEN, and whether source maps are being uploaded during the build step."

**PostHog events aren't appearing**
> "Claude, I performed actions in the app but PostHog shows no events. Check: (1) Is the NEXT_PUBLIC_POSTHOG_KEY correct? (2) Is PostHog initialized in the app layout? (3) Are events being fired from client components (PostHog client SDK runs in the browser)? (4) Is there an ad blocker interfering?"

**Session replay isn't recording**
> "Claude, PostHog session replay shows no sessions. Check the session replay configuration, the sample rate, and whether the recording script is loading on the page. Ad blockers can also block session recording."

**Feature flag always returns the same value**
> "Claude, the feature flag seems stuck -- it always shows the old layout regardless of the PostHog setting. Check whether the flag is being evaluated server-side or client-side, whether the PostHog client is properly identified, and whether the flag name matches exactly between the code and the PostHog dashboard."

**Tests fail after adding monitoring**
> "Claude, existing tests are failing after adding Sentry and PostHog. The test environment probably doesn't have the monitoring environment variables. Mock or disable Sentry and PostHog in the test environment so they don't interfere with tests."

---

## Reflection

You now have visibility into your production application. When something breaks, Sentry tells you -- with full context about what happened, who was affected, and where in the code the problem lives. When you want to understand how people use your product, PostHog shows you -- which features get used, where people drop off, and what the experience looks like through session replay.

This is the difference between operating a product and just having a product deployed. Operating means you're watching, learning, and responding. You know your application is healthy not because you assume it is, but because you have data that proves it.
