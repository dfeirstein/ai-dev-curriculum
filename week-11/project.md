# Project: TeamTask Pro — Dashboard, Admin, and v1 Launch

**What you're building:** The admin and analytics layer for TeamTask Pro -- org-level dashboards, admin panel, activity log, and data export. Then you ship v1.

**Time estimate:** 8-10 hours

**What you'll need open:** Ghostty (your terminal) and a web browser

---

## The Brief

TeamTask Pro has all its core features. This week you're adding the layer that makes it manageable at scale: dashboards that show the health of an organization, admin tools for managing members and subscriptions, an activity log for accountability, and data export so users can get their data out.

When you're done, you'll deploy v1 -- the first complete, launchable version of TeamTask Pro.

---

## Phase 1: Research

Start Claude Code and research the dashboard and admin approach:

> "Claude, I have a multi-tenant project management app (TeamTask Pro) with orgs, projects, tasks, members, Stripe subscriptions, and notifications. I want to add an org-level dashboard with charts, an admin panel for managing members and viewing subscription status, an activity log, and CSV data export. What's the best approach? What charting library should we use for Next.js?"

Follow up:

> "What aggregation queries will I need for the dashboard? Think about tasks by status, tasks completed over time, and member activity."

> "How should the activity log be structured in the database? What events should we track?"

> "What's the simplest way to generate and download CSV files in a Next.js app?"

---

## Phase 2: Plan

> "Claude, plan the complete admin layer for TeamTask Pro. I need: (1) an org dashboard page with charts showing tasks by status, tasks completed per week, member activity, and project progress, (2) an admin panel with member management (invite, change role, remove), usage stats, and subscription status linked to Stripe, (3) an activity log that records all important actions with filtering, (4) CSV export for tasks, projects, and activity. Plan the database changes, API routes, and UI components."

Review the plan. Key questions:

- What new database tables or columns are needed (especially for the activity log)?
- What API routes are needed for aggregation data?
- How do the charts get their data -- server-side queries or client-side fetching?
- Who can access the admin panel (admin role only or all members)?

---

## Phase 3: Execute

### Step 1: Activity Log Infrastructure

Build the foundation first -- the activity log captures events that feed the dashboard.

> "Claude, create an activity log system for TeamTask Pro. Add an activity_logs table with columns for: organization ID, user ID, action type (created, updated, deleted, completed), target type (task, project, member), target ID, target name, details (JSON for context like 'changed status from open to completed'), and timestamp. Create a utility function that logs activity from anywhere in the app. Hook it into existing API routes so that creating, updating, completing, and deleting tasks and projects automatically creates log entries."

Test it:
- Create a task -- does an activity log entry get created?
- Complete a task -- does another entry appear?
- Create a project -- logged?

### Step 2: Org-Level Dashboard

> "Claude, build an org dashboard page at /dashboard for TeamTask Pro. Include these sections: (1) Summary cards at the top showing total tasks, completed tasks, active projects, and active members. (2) A bar chart showing tasks by status (open, in progress, completed) across the org. (3) A line chart showing tasks completed per week for the last 8 weeks. (4) A horizontal bar chart showing each member's task completion count for the current month. (5) Project progress cards showing each project's name, task count, and completion percentage as a progress bar. Use a charting library that works well with Next.js. Make the dashboard visually clean with good spacing and consistent colors."

Test it:
- Does the dashboard load with real data?
- Do the charts display correctly?
- Create and complete some tasks -- do the numbers update?
- Does it look clean and professional?

Give feedback:

> "Claude, the charts need better labels. Add axis labels, data point values on the bars, and a legend where appropriate."

> "Claude, the summary cards should show a comparison to last week -- like 'Completed: 23 (+5 from last week)' with green for up and red for down."

### Step 3: Admin Panel

> "Claude, build an admin panel at /admin for TeamTask Pro. Restrict access to org admins only -- redirect non-admins to the dashboard. Include: (1) A member management table showing name, email, role, join date, last active date, and actions (change role, remove member). Add an 'Invite Member' button that opens a modal for entering an email and selecting a role. (2) A usage statistics section showing total members vs. plan limit, total projects, total tasks, and storage usage. (3) A subscription section showing the current plan name, billing cycle, next renewal date, and a button that links to the Stripe customer portal for managing the subscription."

Test it:
- Can an admin see the admin panel?
- Does a non-admin get redirected?
- Does the member list show correct data?
- Does the Stripe portal link work?

### Step 4: Activity Log Page

> "Claude, build an activity log page at /activity for TeamTask Pro. Show a reverse-chronological feed of all organization activity. Each entry should display: the user's name, what action they took, what they acted on (with a link to the target if it still exists), and a relative timestamp ('2 hours ago'). Add filters for: time range (today, this week, this month, all time), action type (created, updated, completed, deleted), and specific user. Include pagination -- load 50 entries at a time with a 'Load More' button."

Test it:
- Does the activity log show real events?
- Do the filters work correctly?
- Does pagination work?
- Are the timestamps accurate?

### Step 5: CSV Data Export

> "Claude, add CSV data export to TeamTask Pro. On the dashboard or a dedicated /export page, add export buttons for: (1) 'Export Tasks' -- downloads all org tasks with columns: task name, project name, status, assignee, created date, completed date, due date. (2) 'Export Projects' -- downloads all org projects with columns: project name, member count, total tasks, completed tasks, completion percentage, created date. (3) 'Export Activity' -- downloads the activity log with columns: date, user, action, target type, target name, details. The CSV should download immediately as a file. Include proper headers and handle special characters in data."

Test it:
- Download each CSV file
- Open them in a spreadsheet application (Excel, Google Sheets, Numbers)
- Verify the data is correct and the columns make sense
- Check that special characters (commas, quotes in task names) don't break the format

### Step 6: Navigation and Polish

> "Claude, update the TeamTask Pro navigation to include links to Dashboard, Projects, Activity, and Admin (admin-only). Add the current page highlight. Make sure the navigation is consistent across all pages. Add a breadcrumb trail on subpages."

> "Claude, add empty states to the dashboard -- if the org has no tasks yet, show a helpful message with a call to action to create the first project, instead of empty charts."

> "Claude, review the entire app for visual consistency. Make sure spacing, typography, colors, and component styles are consistent across all pages -- dashboard, admin, activity, projects, tasks."

### Step 7: Quality Gates

> "Claude, run npm run lint and fix any issues."

> "Claude, run npm run typecheck and fix any issues."

> "Claude, run npm run build and make sure it completes without errors."

### Step 8: Ship v1

> "Claude, commit all changes with a clear message about the dashboard and admin layer, push to GitHub, and deploy to Vercel."

After deployment, run through the complete user journey on the live URL:

1. Sign up as a new user
2. Create an organization
3. Invite a member (use a second email if possible)
4. Create a project
5. Add tasks to the project
6. Complete some tasks
7. Check the dashboard -- do the charts reflect your activity?
8. Check the activity log -- is everything recorded?
9. Export tasks as CSV -- does the file download correctly?
10. Visit the admin panel -- does member management work?
11. Check the subscription section -- does it link to Stripe?

If everything works, v1 is shipped.

---

## Acceptance Criteria

Your project is complete when all of these are true:

- [ ] Org-level dashboard shows summary cards, task status chart, weekly completion trend, and member activity
- [ ] Charts use real data from the database and display correctly
- [ ] Admin panel is restricted to org admins
- [ ] Admin panel shows member management with invite, role change, and remove capabilities
- [ ] Admin panel shows subscription status with link to Stripe customer portal
- [ ] Activity log records task and project events automatically
- [ ] Activity log page shows a filterable, paginated feed of org activity
- [ ] CSV export works for tasks, projects, and activity log
- [ ] Exported CSV files open correctly in a spreadsheet application
- [ ] Navigation includes all major sections with proper access control
- [ ] Empty states are handled gracefully
- [ ] `npm run lint` passes
- [ ] `npm run typecheck` passes
- [ ] `npm run build` completes without errors
- [ ] App is deployed and functional on Vercel
- [ ] Code is on GitHub

---

## Stretch Goals

**Real-Time Dashboard Updates**
> "Claude, make the dashboard update in real time. When another team member completes a task, the charts should update without refreshing the page. Use server-sent events or polling."

**Custom Date Range Picker**
> "Claude, add a date range picker to the dashboard so users can view metrics for any custom time period, not just preset ranges."

**PDF Report Generation**
> "Claude, add a 'Generate Report' button on the dashboard that creates a PDF document with all the current dashboard data, charts included, suitable for sharing with stakeholders."

**Role-Based Dashboard Views**
> "Claude, create different dashboard views based on role. Admins see the full org dashboard. Regular members see a personal dashboard focused on their own tasks, deadlines, and activity."

---

## Troubleshooting

**Charts aren't rendering**
> "Claude, the dashboard charts aren't showing up. Check the charting library installation, the data fetching for the charts, and whether the component is rendering client-side (charts typically need 'use client')."

**Aggregation data is wrong**
> "Claude, the task count on the dashboard doesn't match what I see in the projects. Check the aggregation queries -- make sure they're scoped to the current organization and using the right filters."

**CSV download isn't working**
> "Claude, clicking the export button doesn't download a file. Check the API route for CSV generation, the response headers (should be 'text/csv' with Content-Disposition), and the client-side download trigger."

**Admin panel accessible to non-admins**
> "Claude, regular members can see the admin panel. Check the role-based access control on the /admin route -- it should check the user's role in the current org and redirect non-admins."

**Activity log not recording events**
> "Claude, I created tasks but nothing shows up in the activity log. Check that the activity logging function is being called in the task creation API route, and that the activity_logs table was created in the database."

---

## Reflection

This is v1. A complete, multi-tenant SaaS application with:

- User authentication with multiple sign-in methods
- Organization-based multi-tenancy
- Project and task management
- Stripe payment integration with subscription tiers
- Email and in-app notifications
- Org-level dashboards with data visualization
- Admin tools for member and subscription management
- Activity logging and audit trail
- Data export

You directed the construction of all of this. You didn't write the code. You understood the concepts, described what you wanted, verified the output, and shipped it. That's the AI-native builder workflow, and you just used it to build something substantial.

Take a moment. Share the URL. This is real.
