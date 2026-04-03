# Week 11: TeamTask Pro — Dashboard and Admin — Instructor Guide

## Week at a Glance
- Theme: Data visualization, admin panels, CSV export, and the V1 launch milestone. Students transform TeamTask Pro from a functional tool into a product that looks and feels complete.
- What students ship: A deployed TeamTask Pro with an org dashboard, admin panel, activity logs, and CSV data export. This is the **V1 launch**.
- Estimated hours: 10-14
- Difficulty: 3

## Pacing Guide
- **Day 1-2:** Students read guide.md and research dashboard design patterns. They should direct Claude Code to build the dashboard page with 5 key metrics (tasks by status, completion trends, active members, overdue tasks, project progress). By end of Day 2, the dashboard should render real data from the database.
- **Day 3-4:** Admin panel (member management, role changes, org settings), activity logs, and CSV export. Students direct Claude Code to implement each feature, verify it works with real data, then move to the next.
- **Day 5:** Polish, responsive design pass, final deploy to Vercel. **V1 celebration.** Students should ship a link they are proud to share.

## Getting Students Started
- Kick off with energy. This is the week everything comes together. Remind students that after 10 weeks, they are launching a real product.
- Common confusion: students are unsure which metrics to put on the dashboard. Point them to the "Key Question Test" in guide.md -- if a metric does not lead to a decision, it does not belong.
- Students should have open: TeamTask Pro repo in their terminal, Claude Code CLI running, the deployed app in a browser tab, and at least one org with realistic sample data (multiple members, projects, tasks in various states).

## Check-in Points
- **After guide.md:** Verify the student can articulate which 5 metrics they will show and why. If they list more than 7, push them to cut. More metrics is not better.
- **Mid-project checkpoint:** By end of Day 3, the dashboard should display real data and the admin panel should allow role changes. If the student is behind, have them skip the activity log and focus on dashboard + admin + export.
- **Pre-ship review:** Dashboard renders without errors, admin panel works for org owners only, CSV export downloads a valid file, the app is deployed and the URL works. The student should demo the full flow: log in as an org owner, view dashboard, manage a member, export data.

## Common Pitfalls
- **Dashboard shows mock/hardcoded data instead of real queries.** Students sometimes accept Claude Code's first pass which uses placeholder data. Have them verify by creating a new task and checking if the dashboard updates.
- **Chart library bloat.** Students may ask Claude Code to install heavy charting libraries when simple bar charts with Tailwind or a lightweight library like Recharts would suffice. Steer toward simplicity.
- **Admin panel lacks role-based access.** The admin panel must only be visible to org owners/admins. Check that a regular member cannot access it. Students forget to test from a non-admin account.
- **CSV export produces malformed files.** Have students open the exported CSV in a spreadsheet app. Common issues: missing headers, commas inside fields not escaped, dates in inconsistent formats.
- **Skipping the celebration.** This is V1. Do not let it pass without acknowledgment. The student built a multi-tenant SaaS app from scratch using AI tools. That is worth pausing on.

## How to Know the Student Gets It
- The student chooses metrics deliberately and can explain why each one matters to an org owner.
- The student tests the dashboard with real data, not just visually inspects it.
- The student thinks about access control without being prompted ("only admins should see this").
- The student is excited about the deploy. They want to show someone the link.
- **Red flags:** Student ships a dashboard with 15 metrics and no hierarchy. Student cannot explain what the CSV export contains. Student has not tested with multiple user roles.

## Support Strategies
- If a student is overwhelmed by the number of features, have them prioritize: dashboard first, then CSV export, then admin panel. A shipped dashboard with export is better than a half-built admin panel.
- If charts are not rendering, have the student ask Claude Code to add a simple table view first, then layer in charts. Tables always work; charts sometimes have configuration issues.
- For students who finish early, this is a great time to have them go back and polish earlier features -- fix edge cases, improve error messages, tighten up the UI.

## Differentiation
- **Moving fast?** Add a date range picker to the dashboard so metrics can be filtered by week/month/quarter. Add a PDF export option alongside CSV. Build a "getting started" onboarding checklist for new org members.
- **Struggling?** Cut the activity log entirely. Focus on a dashboard with 3 metrics (tasks by status, overdue tasks, active members) plus CSV export. The admin panel can be a simple member list with remove functionality only.
- **Minimum viable ship:** A dashboard page that shows tasks by status and a working CSV export button. Deployed to Vercel.
