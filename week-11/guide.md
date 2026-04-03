# Week 11 Guide: Dashboard and Admin

TeamTask Pro has users, organizations, projects, tasks, payments, and notifications. That's a lot of moving parts. But right now, nobody can see the big picture. An org owner can't answer basic questions: How many tasks did the team complete this week? Who's most active? Which projects are falling behind? Is our subscription being used?

Dashboards answer these questions at a glance. Admin panels let org owners manage their team. Activity logs provide accountability. Data export gives users control over their own data.

This is the layer that separates a toy from a product.

---

## Part 1: Dashboard Design -- What Metrics Matter

### The Purpose of a Dashboard

A dashboard exists to answer questions without making people dig. When an org owner opens TeamTask Pro, they should immediately understand the health of their organization.

The biggest mistake in dashboard design is showing everything. More data is not better. Better data is better. A dashboard with 30 metrics is a spreadsheet. A dashboard with 5 well-chosen metrics is a command center.

### Choosing Metrics

For a project management tool like TeamTask Pro, the metrics that matter fall into three categories:

**Progress metrics** -- Are we getting things done?
- Tasks completed this week/month
- Tasks by status (open, in progress, completed)
- Project completion percentage

**Activity metrics** -- Is the team engaged?
- Active members (who logged in or made changes recently)
- Tasks created vs. tasks completed (is work piling up or getting done?)
- Recent activity (what happened today/this week)

**Health metrics** -- Are there problems?
- Overdue tasks (things past their deadline)
- Stalled projects (no activity in X days)
- Subscription status (are we on a plan that fits our usage?)

### The Key Question Test

Before adding any metric to a dashboard, ask: "What decision would someone make based on this number?" If the answer is "nothing" or "I don't know," the metric doesn't belong on the dashboard.

- "Tasks completed this week: 47" -- Decision: The team is productive. Stay the course.
- "Tasks overdue: 12" -- Decision: Something is stuck. Let's look at which projects are behind.
- "Average task age: 4.2 days" -- Decision: Unclear. What do I do with that? Probably doesn't belong front and center.

---

## Part 2: Data Visualization -- Charts and When to Use Them

### Why Charts Exist

Numbers alone are hard to interpret. "Completed: 47, In Progress: 23, Open: 15" requires mental math. A bar chart showing those same numbers lets you see the proportions instantly. Visualization turns data into understanding.

### Chart Types and Their Sweet Spots

**Bar charts** -- Comparing quantities across categories.
- Tasks by status (open, in progress, completed)
- Tasks per project
- Activity by team member

Bar charts are the workhorse of dashboards. When in doubt, use a bar chart. They're intuitive, easy to read, and work for most comparisons.

**Line charts** -- Showing trends over time.
- Tasks completed per day/week over the last month
- Member activity over time
- Project progress over time

Line charts answer "is this going up or down?" They're essential for spotting trends -- are we completing more tasks each week, or fewer?

**Pie/donut charts** -- Showing proportions of a whole.
- Task status breakdown (what percentage is complete?)
- Tasks by project (where is the effort going?)

Use pie charts sparingly. They work well when you have 3-5 slices. With more than that, they become unreadable. A bar chart is almost always a better alternative.

**Progress bars** -- Showing completion toward a goal.
- Project completion (65% of tasks done)
- Sprint progress
- Subscription usage (using 8 of 10 seats)

### Design Principles for Charts

- **Label everything.** Every axis, every bar, every slice needs a label. If someone has to guess what they're looking at, the chart has failed.
- **Use consistent colors.** Pick a color for "completed" (green), "in progress" (blue), "overdue" (red) and use them everywhere. Consistency reduces cognitive load.
- **Keep it simple.** No 3D effects, no gratuitous animation, no decorative elements. Charts are tools, not art.
- **Show the number.** Always display the actual value alongside the visual. A bar chart shows proportions; the number on top of each bar gives precision.

---

## Part 3: Admin Panels -- What Admins Need

### The Admin's Job

An org admin in TeamTask Pro needs to:

1. **See who's in the organization** -- all members, their roles, when they joined, when they were last active.
2. **Manage members** -- invite new members, change roles, remove members.
3. **Understand usage** -- how many tasks, projects, and members exist. Is the org active or dormant?
4. **Monitor the subscription** -- what plan are we on, when does it renew, how many seats are we using.
5. **Review activity** -- what's been happening? Who did what?

### Member Management

The admin panel should show a table of all organization members with:
- Name and email
- Role (admin, member)
- Join date
- Last active date
- Actions (change role, remove)

Inviting new members should be straightforward -- enter an email, choose a role, send an invite.

### Usage Statistics

A summary section showing:
- Total members (and how many are active this week)
- Total projects
- Total tasks (broken down by status)
- Storage or feature usage relative to plan limits

### Subscription Status

Show the current plan, billing cycle, next renewal date, and a link to the Stripe customer portal for managing the subscription. This connects to the Stripe integration from Week 9.

---

## Part 4: Aggregation Queries -- Conceptual Understanding

### What Aggregation Means

Your database has individual records: tasks, users, projects, activity entries. Dashboards need summaries: "how many tasks per project," "total completed this week," "most active member."

Aggregation is the process of taking many individual records and producing summary statistics. You don't need to write these queries yourself -- Claude Code handles that -- but understanding the concepts helps you describe what you want.

### Common Aggregation Operations

**Count** -- How many of something exist?
- "How many tasks are in each status?" -- Count tasks, grouped by status.
- "How many members are in the org?" -- Count members.

**Sum** -- What's the total?
- "How many tasks were completed across all projects?" -- Sum of completed tasks.

**Group By** -- Break down a count or sum by category.
- "Tasks per project" -- Count tasks, grouped by project.
- "Completions per member" -- Count completed tasks, grouped by who completed them.

**Filter + Aggregate** -- Narrow down before summarizing.
- "Tasks completed this week" -- Filter to this week's tasks, then count where status is "completed."
- "Active members this month" -- Filter activity to this month, then count unique members.

### When Directing Claude Code

You describe the question, not the query:

> "Show me how many tasks are in each status for the current organization"

> "I need a chart showing tasks completed per week for the last 8 weeks"

> "Show each member's name and how many tasks they completed this month"

Claude Code translates these into the right database queries. Your job is knowing what questions to ask.

---

## Part 5: Activity Logs -- The Audit Trail

### Why Activity Logs Matter

Activity logs answer "who did what, when." They serve three purposes:

1. **Accountability** -- If a task gets deleted or a member gets removed, there's a record of who did it and when.
2. **Debugging** -- When something goes wrong, the activity log helps you trace back to what happened.
3. **Awareness** -- Team members can see what's been happening in the org without asking around.

### What to Log

Not everything needs to be logged. Focus on actions that change state:

- Task created, updated, completed, deleted
- Project created, archived, deleted
- Member invited, role changed, removed
- Subscription changed

Each log entry should capture:
- **Who** -- Which user performed the action
- **What** -- What action they took (created, updated, deleted)
- **Target** -- What they acted on (which task, which project, which member)
- **When** -- Timestamp
- **Details** -- Any relevant context (e.g., "changed status from 'open' to 'completed'")

### Displaying the Activity Log

Activity logs are typically shown as a reverse-chronological feed -- newest first. Each entry is a single line: "Alice completed task 'Design mockups' in Project Alpha -- 2 hours ago."

Filtering matters here. Let admins filter by:
- Time range (today, this week, this month)
- Action type (creates, updates, deletes)
- User (what did Alice do?)
- Target (what happened to Project Alpha?)

---

## Part 6: Data Export -- Users Own Their Data

### Why Data Export Matters

Users should be able to get their data out of your application. This is both a trust issue and, in some jurisdictions, a legal requirement.

CSV (comma-separated values) is the universal export format. Every spreadsheet application (Excel, Google Sheets) can open a CSV file. It's simple, portable, and human-readable.

### What to Export

For TeamTask Pro, users should be able to export:
- All tasks (with project name, status, assignee, dates)
- All projects (with member count, task count, status)
- Activity log (with timestamps, actions, actors)

### How Export Works (Conceptual)

1. User clicks "Export" and chooses what to export (tasks, projects, or activity).
2. The server queries the database for the relevant data, scoped to their organization.
3. The server formats the data as CSV.
4. The browser downloads the file.

It's a simple flow, but it's one of those features that makes users trust your product. "I can always get my data out" is a powerful reassurance.

---

## Part 7: The v1 Mindset

### What "Good Enough to Launch" Means

v1 is not "perfect." v1 is "complete enough that someone can use it and get value from it."

Here's what v1 of TeamTask Pro should have:
- Users can sign up, create orgs, invite members
- Teams can create projects and tasks
- Tasks can be assigned, tracked, and completed
- Org owners can see dashboards and manage their team
- Payments work (free and paid tiers)
- Notifications keep people informed
- Data can be exported
- The app is stable and deployed

Here's what v1 does NOT need:
- Every possible feature
- Perfect design on every screen
- Handling every edge case
- Mobile optimization (nice to have, not required)
- Advanced analytics or reporting

### The Trap of "Just One More Feature"

The most common reason products never launch is that the builder keeps adding features. "Let me just add..." is the enemy of shipping. v1 exists to learn. Ship it, see how people react, and improve based on real feedback. The features you think users want are often wrong. The only way to find out is to launch.

### Celebrating the Milestone

Seriously -- take a moment when v1 is deployed. You've directed the construction of a multi-tenant SaaS application with authentication, payments, notifications, dashboards, and admin tools. That's not a tutorial project. That's a real product. Most people who want to build software never get this far.

---

## What's Next

Head to [project.md](project.md) to build the admin layer and ship v1 of TeamTask Pro.
