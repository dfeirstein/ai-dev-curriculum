# Week 13 Guide: Monitoring and Analytics

Your application is deployed. People can use it. But you're flying blind. Right now, if an error happens in production, you don't know about it unless a user complains. If a feature is confusing and nobody uses it, you have no way to tell. If the signup flow is broken on Safari, you might not discover that for weeks.

This week you add two kinds of visibility: monitoring (is the application working?) and analytics (how are people using it?). These are different questions with different tools.

---

## Part 1: Why Monitoring Matters

### The Invisible Failures

In development, you see every error. It appears in your terminal, in your browser console, on the page itself. In production, errors happen on someone else's computer, in a browser you don't have, at 3am when you're asleep.

Without monitoring, here's what happens:
1. A user encounters an error
2. They get frustrated
3. They leave
4. You never find out

With monitoring, here's what happens:
1. A user encounters an error
2. Sentry captures it with full context
3. You get notified
4. You fix it before the next user hits it

The difference is the difference between a professional product and a demo.

### What "Monitoring" Means

Monitoring answers: **Is my application working correctly right now?**

It's not about performance metrics or user behavior. It's about errors, crashes, and failures. When something goes wrong in production, monitoring tells you immediately -- what broke, where it broke, which users were affected, and what they were doing when it happened.

---

## Part 2: Sentry Deep Dive

### Error Tracking

Sentry's core job is catching errors. When an exception occurs anywhere in your application -- client-side in the browser, server-side in an API route, in a background job -- Sentry captures it and sends a report.

Each error report includes:
- **The error message and stack trace** -- What went wrong and where in the code
- **Browser and OS information** -- What device and browser the user was on
- **User context** -- Which user was affected (if you configure user identification)
- **Breadcrumbs** -- A timeline of events leading up to the error (page navigations, button clicks, API calls)
- **Request data** -- For server errors, the URL, method, headers, and body of the request that triggered the error

This context is what makes Sentry valuable. "TypeError: Cannot read property 'name' of undefined" is useless on its own. "TypeError: Cannot read property 'name' of undefined -- on the task detail page, for user alice@example.com, after she clicked 'Mark Complete' on a task that was already deleted" -- that's actionable.

### Source Maps

When your Next.js application is deployed, the code is compiled, minified, and bundled. An error in production says "error at line 1, column 47823 of chunk-abc123.js." That tells you nothing.

Source maps are files that map the compiled code back to your original source files. With source maps configured, Sentry shows you "error at line 42 of src/app/api/tasks/route.ts" -- the exact file and line in your actual code.

Without source maps, production errors are nearly useless. With them, you can jump straight to the problem.

### Alerts

Sentry can notify you when errors happen. You configure alert rules:
- **First occurrence** -- Get notified the first time a new error type appears
- **Frequency** -- Get notified when an error occurs more than N times in a time window
- **Regression** -- Get notified when an error you previously resolved comes back

Alerts can go to email, Slack, or other channels. The goal is knowing about production problems quickly, not checking a dashboard every hour.

### Release Tracking

Sentry can correlate errors with deploys. When you deploy a new version, you tell Sentry "this is release v1.2.3." If new errors start appearing after that release, Sentry highlights the connection. This helps you answer: "Did our last deploy break something?"

Release tracking also enables the concept of "resolving" errors. You fix a bug, deploy, and mark the error as resolved. If it comes back in a future release, Sentry alerts you that it regressed.

---

## Part 3: PostHog Deep Dive

### What Analytics Means

Analytics answers a different question than monitoring. Monitoring asks "is it working?" Analytics asks "how is it being used?"

Product analytics tells you:
- Which features people use (and which they ignore)
- Where people get stuck or confused
- How users progress through your application
- Whether changes you make improve or worsen the user experience

### Event Tracking

PostHog tracks events -- specific user actions. Each event has a name and optional properties:

- **Event:** `task_created`
  **Properties:** `project_name`, `has_due_date`, `has_assignee`

- **Event:** `signup_completed`
  **Properties:** `signup_method` (email or oauth), `referral_source`

- **Event:** `subscription_upgraded`
  **Properties:** `from_plan`, `to_plan`, `billing_cycle`

Events are the raw data. Everything else in PostHog (funnels, trends, cohorts) is built on top of events.

### Funnels

A funnel shows you how users progress through a sequence of steps. For TeamTask Pro:

```
Signup → Create Org → Create Project → Create Task → Complete Task
  100%      72%          58%              41%           23%
```

This tells a story: 100 people sign up, but only 23 actually complete a task. Where are they dropping off? Between "Create Org" and "Create Project" you lose 14% -- maybe the project creation flow is confusing. Between "Create Task" and "Complete Task" you lose 18% -- maybe people create tasks but never come back.

Funnels turn guesses into data. Instead of "I think the onboarding is fine," you know "42% of users never create their first project."

### Feature Flags

Feature flags let you control which users see which features, without deploying new code. You wrap a feature in a flag, and PostHog decides at runtime whether to show it.

Use cases:
- **Gradual rollout** -- Release a new dashboard design to 10% of users first. If metrics are good, increase to 50%, then 100%.
- **Beta features** -- Give early access to specific users or organizations.
- **Kill switch** -- If a new feature causes problems, disable it instantly without deploying.
- **A/B testing** -- Show version A to half your users and version B to the other half. Measure which performs better.

Feature flags are how production teams ship safely. Instead of deploying to everyone and hoping for the best, you deploy to a small group, verify it works, and gradually expand.

### Session Replay

Session replay records what users see and do. You can watch a recording of a user's session: every page they visited, every button they clicked, every form they filled out, every error they saw.

This is incredibly powerful for understanding user experience. When your funnel shows a drop-off at the project creation step, you can watch session replays of users at that step and see exactly what confused them. Did they not find the button? Did they fill out the form wrong? Did the page error out?

Session replay is also invaluable for debugging. When a user reports "it doesn't work," you can watch their session and see exactly what happened.

---

## Part 4: Defining a Tracking Plan

### What a Tracking Plan Is

A tracking plan is a document that lists every event you'll track, what properties each event includes, and why you're tracking it. You define this BEFORE implementing analytics.

Why plan first? Because adding random events without a plan leads to messy, unusable data. You end up with 200 events, half of which are redundant, and you still can't answer the questions that matter.

### The Questions-First Approach

Start with the questions you want to answer:

1. **Are people signing up and onboarding successfully?**
   Events: `signup_started`, `signup_completed`, `org_created`, `first_project_created`, `first_task_created`

2. **Which features are people using?**
   Events: `task_created`, `task_completed`, `project_created`, `dashboard_viewed`, `csv_exported`, `member_invited`

3. **Are paid features driving upgrades?**
   Events: `paywall_shown`, `upgrade_started`, `upgrade_completed`, `subscription_canceled`

4. **Where do people get stuck?**
   Events: `error_shown`, `form_abandoned`, `help_clicked`

### Tracking Plan for TeamTask Pro

Here's the tracking plan you'll implement:

| Event | Properties | Why |
|-------|-----------|-----|
| `signup_completed` | method (email/oauth) | Measure signup conversion |
| `org_created` | member_count | Track onboarding progress |
| `project_created` | org_id | Measure feature adoption |
| `task_created` | has_due_date, has_assignee, project_id | Understand task creation patterns |
| `task_completed` | time_to_complete, project_id | Measure productivity |
| `member_invited` | role | Track team growth |
| `dashboard_viewed` | - | Measure dashboard engagement |
| `csv_exported` | export_type (tasks/projects/activity) | Measure data export usage |
| `subscription_upgraded` | from_plan, to_plan | Track revenue events |
| `subscription_canceled` | reason (if collected) | Understand churn |
| `paywall_shown` | feature_name | Measure upgrade triggers |

### What Not to Track

Not everything should be an event. Don't track:
- Every page view (PostHog captures these automatically)
- Every keystroke or mouse movement (this is surveillance, not analytics)
- Personally identifiable information beyond what's necessary (no tracking email contents, personal notes, etc.)

---

## Part 5: Privacy -- What to Track vs. What's Creepy

### The Line

Analytics exist to improve the product. They should answer "how can we make this better?" not "what is this specific person doing?"

Good tracking:
- "47% of users complete onboarding within 24 hours"
- "The CSV export feature is used by 12% of active orgs"
- "Users who invite at least 2 members are 3x more likely to upgrade"

Creepy tracking:
- Recording everything a user types in task descriptions
- Tracking exactly how long someone stares at each page
- Correlating user behavior with personal demographics without consent

### Practical Guidelines

1. **Only track actions, not content.** Track that a task was created, not what was written in it.
2. **Aggregate, don't individualize.** Care about patterns across users, not individual behavior.
3. **Be transparent.** If you track analytics, say so in your privacy policy.
4. **Respect opt-out.** Give users the ability to opt out of analytics. PostHog supports this.
5. **Minimize properties.** Only include event properties that help answer your questions. More data isn't better if it's not useful.

### Legal Considerations

Different jurisdictions have different privacy laws (GDPR in Europe, CCPA in California). The practical takeaway: tell users what you collect, let them opt out, and don't collect more than you need. For a project like TeamTask Pro, staying on the right side of privacy is straightforward if you follow the guidelines above.

---

## What's Next

Head to [project.md](project.md) to instrument TeamTask Pro with Sentry and PostHog.
