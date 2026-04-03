# Project: TeamTask Pro -- Notifications and Email

**What you're building:** Adding transactional email via Resend and in-app notifications to TeamTask Pro. Welcome emails, invitation emails, task assignment notifications, a notification bell, and user notification preferences.

**Time estimate:** 10-12 hours

**What you'll need open:** Ghostty (your terminal), a web browser, and the Resend dashboard.

---

## The Brief

TeamTask Pro is almost complete. It has auth, organizations, projects, tasks, payments, and feature gating. But it doesn't communicate with users proactively. This week you add the communication layer that makes the difference between a functional tool and a polished product.

After this week, TeamTask Pro will welcome new users, email invitations, notify people about their tasks, and let users control their notification preferences. It will be a complete, production-quality SaaS application.

---

## Phase 1: Research

> "Claude, I need to add transactional email and in-app notifications to my Next.js SaaS app. For email, I want to use Resend with React Email templates. For in-app notifications, I want the bell icon pattern with an unread count. What's the best architecture for a notification system that handles both email and in-app?"

> "Claude, how should I structure the notification service? I want a central place that receives events (like 'task assigned') and decides which notifications to send (email, in-app, or both) based on the event type and user preferences."

> "Claude, for in-app notifications, should I use polling or real-time updates? What's the tradeoff? How often should I poll for new notifications?"

> "Claude, how does Resend handle email deliverability? Do I need to configure SPF and DKIM? What if I want to send from notifications@teamtaskpro.com?"

### Set Up Resend

Before building:

1. Create a Resend account at [resend.com](https://resend.com) if you don't have one
2. Create an API key in the Resend dashboard
3. Note: on the free tier, you can only send to the email address you signed up with -- that's fine for testing

---

## Phase 2: Plan

> "Claude, plan the notifications and email integration for TeamTask Pro. Here's the spec:
>
> **Email (via Resend + React Email):**
> - Welcome email on signup (greeting, getting started link)
> - Invitation email when someone is invited to an org (org name, role, accept link)
> - Task assignment email when a task is assigned (task title, project name, link to task)
> - Payment failed email to org owner (prompt to update payment method)
>
> **In-App Notifications:**
> - Notification bell in nav bar with unread count badge
> - Dropdown showing recent notifications (title, message, time ago, read/unread)
> - Click notification to navigate to relevant page and mark as read
> - 'Mark all as read' button
> - Poll for new notifications every 30 seconds
>
> **Notification Events:**
> - User signs up -> welcome email
> - User invited to org -> invitation email
> - Invitation accepted -> in-app notification to org admins
> - Task assigned -> email + in-app notification to assignee
> - Task status changed -> in-app notification to task creator
> - Payment failed -> email + in-app notification to org owner
>
> **User Preferences:**
> - Settings page to toggle email notifications by type
> - Respect preferences before sending email
> - Unsubscribe link in every email
>
> **Database changes:**
> - notifications table (recipientId, type, title, message, link, read, createdAt)
> - notification_preferences table (userId, taskAssignmentEmail, taskReminderEmail, weeklyDigestEmail, paymentAlertEmail)
>
> Plan the database changes, notification service architecture, email templates, UI components, and API routes."

Review the plan. Key questions:

- Is there a central notification service, or are notification triggers scattered?
- How are user preferences checked before sending email?
- What does the notification dropdown look like?
- How does polling work -- what API endpoint, what frequency?
- Are email templates consistent in design?

---

## Phase 3: Execute

### Step 1: Environment Setup

> "Claude, add Resend environment variables: RESEND_API_KEY, RESEND_FROM_EMAIL (use 'onboarding@resend.dev' for testing or your custom domain). Install the resend and @react-email/components packages. Update .env.local and .env.example."

### Step 2: Database Changes

> "Claude, add the notification tables:
> 1. notifications table: id, recipientId (references users), type (enum: task_assigned, task_status_changed, member_joined, payment_failed), title, message, link, read (boolean, default false), createdAt
> 2. notification_preferences table: userId (references users), taskAssignmentEmail (boolean, default true), taskReminderEmail (boolean, default true), weeklyDigestEmail (boolean, default true), paymentAlertEmail (boolean, default true)
> Run the migration."

### Step 3: Email Templates

> "Claude, create React Email templates for TeamTask Pro:
> 1. WelcomeEmail: greeting with user's name, brief welcome message, 'Get Started' button linking to the dashboard. Clean design with TeamTask Pro branding.
> 2. InvitationEmail: 'You've been invited to join [org name]' with the inviter's name, assigned role, and 'Accept Invitation' button.
> 3. TaskAssignedEmail: '[Assigner name] assigned you a task' with task title, project name, and 'View Task' button.
> 4. PaymentFailedEmail: 'Your payment for [org name] failed' with a prompt to update payment method and 'Update Payment' button.
>
> All templates should share a consistent layout: TeamTask Pro header, content area, CTA button, and footer with unsubscribe link."

Preview the templates:

> "Claude, set up a preview route so I can see each email template in the browser during development. I want to view them at /dev/emails/[template-name]."

### Step 4: Email Sending Service

> "Claude, create an email sending service that:
> 1. Takes an email type, recipient, and data (template variables)
> 2. Checks the recipient's notification preferences before sending
> 3. Renders the appropriate React Email template
> 4. Sends via Resend
> 5. Logs every email sent (type, recipient, timestamp)
> 6. Handles errors gracefully (don't crash the app if email fails -- log the error to Sentry)"

### Step 5: Welcome Email

> "Claude, trigger a welcome email when a new user signs up. After successful account creation, call the email service to send the WelcomeEmail template. Also create default notification preferences for the new user (all enabled)."

Test:
- Sign up with a new account
- Check your email for the welcome message
- Verify the content and links are correct

### Step 6: Invitation Email

> "Claude, update the invitation system from Week 8. When an invitation is created, send the InvitationEmail template to the invited person's email address. Include the organization name, the inviter's name, the assigned role, and the accept link."

Test:
- Create an invitation from an org admin account
- Check the invited email for the invitation message
- Click the accept link and verify it works

### Step 7: In-App Notification System

> "Claude, build the in-app notification system:
> 1. Create a notification service function that creates a notification record in the database
> 2. API route GET /api/notifications -- returns the current user's notifications (most recent 20), plus unread count
> 3. API route PATCH /api/notifications/[id] -- marks a notification as read
> 4. API route PATCH /api/notifications/read-all -- marks all notifications as read
> 5. Notification bell component in the nav bar: shows unread count badge, opens a dropdown on click
> 6. Notification dropdown: list of recent notifications with title, message, time ago, read/unread styling
> 7. Clicking a notification calls the mark-as-read API and navigates to the notification's link
> 8. Poll for new notification count every 30 seconds to keep the badge updated"

Test:
- Verify the bell icon appears in the nav
- Manually create a notification in the database to test the UI
- Click the notification and verify it navigates and marks as read

### Step 8: Task Assignment Notifications

> "Claude, when a task is assigned to a user:
> 1. Create an in-app notification for the assignee: 'You've been assigned a task: [task title]' with a link to the task
> 2. Send a task assignment email (check preferences first)
> 3. If the task was previously assigned to someone else and is being reassigned, also notify the previous assignee: 'Task [task title] was reassigned'
> Trigger these from the task update API route when the assigneeId changes."

Test:
- Assign a task to another user
- Check: does an in-app notification appear for the assignee?
- Check: does the assignee receive an email?
- Reassign the task and verify both users are notified

### Step 9: Additional Notification Triggers

> "Claude, add these notification triggers:
> 1. Invitation accepted: create an in-app notification for org admins and owner -- '[Name] joined [org name]'
> 2. Task status changed: create an in-app notification for the task creator -- 'Task [title] moved to [new status]' (only if the person changing status is not the creator)
> 3. Payment failed webhook: create an in-app notification for the org owner and send PaymentFailedEmail"

### Step 10: Notification Preferences

> "Claude, build a notification preferences page at /settings/notifications:
> 1. Show toggles for each email notification type: task assignments, task reminders, weekly digest, payment alerts
> 2. Save preferences to the notification_preferences table
> 3. Load current preferences on page load
> 4. Add an unsubscribe link in every email footer that links to this preferences page
> 5. Before sending any email, check the recipient's preferences -- skip the email if they've opted out"

Test:
- Turn off task assignment emails in preferences
- Assign a task to yourself
- Verify: in-app notification appears, but no email is sent
- Turn it back on, assign again, verify email is sent

### Step 11: Polish

> "Claude, polish the notification experience:
> 1. Add empty state to the notification dropdown ('No notifications yet')
> 2. Add notification grouping -- if there are multiple notifications of the same type, show a count ('3 new task assignments')
> 3. Add a subtle animation when new notifications arrive (badge pulse)
> 4. Make sure all notification links work correctly (including for users who are viewing a different org)
> 5. Add loading states to the notification dropdown"

### Step 12: Quality Gates

> "Claude, run npm run lint and fix any issues."

> "Claude, run npm run typecheck and fix any issues."

> "Claude, run npm run build and make sure it completes without errors."

### Step 13: Deploy

> "Claude, commit all changes, push to GitHub, and deploy to Vercel. Add the Resend environment variables (RESEND_API_KEY, RESEND_FROM_EMAIL) to Vercel."

Test on the live URL:
- Full signup flow with welcome email
- Invitation flow with invitation email
- Task assignment with notification and email
- Notification bell, dropdown, and mark-as-read
- Notification preferences
- Verify all notification links navigate correctly

---

## Acceptance Criteria

Your project is complete when all of these are true:

- [ ] Resend is integrated for transactional email
- [ ] Welcome email is sent on signup
- [ ] Invitation email is sent when inviting a member
- [ ] Task assignment triggers both email and in-app notification
- [ ] Email templates are consistent and professional
- [ ] Notification bell in nav bar shows unread count
- [ ] Notification dropdown shows recent notifications
- [ ] Clicking a notification marks it as read and navigates to the link
- [ ] "Mark all as read" works
- [ ] Notification preferences page lets users toggle email types
- [ ] Email preferences are respected (disabled notifications are not sent)
- [ ] Every email has an unsubscribe link
- [ ] Polling updates the notification count every 30 seconds
- [ ] `npm run lint` passes
- [ ] `npm run typecheck` passes
- [ ] `npm run build` completes without errors
- [ ] Code is on GitHub and deployed to Vercel

---

## Stretch Goals

**Add a Weekly Digest Email**
> "Claude, add a weekly digest email that summarizes activity from the past week: tasks completed, tasks created, new members, upcoming due dates. Create a scheduled function (or document how to set one up with a cron job) that sends this every Monday morning to users who have the weekly digest enabled."

**Add Real-Time Notifications with SSE**
> "Claude, replace polling with Server-Sent Events (SSE) for real-time notifications. When a new notification is created, push it to the recipient's browser immediately. Fall back to polling if SSE is not supported."

**Add Push Notifications**
> "Claude, add browser push notifications using the Web Push API. When a notification is created and the user is not currently on the site, send a browser push notification. Ask for permission on first visit."

**Add @Mentions in Task Comments**
> "Claude, add the ability to @mention team members in task comments. When someone is mentioned, create an in-app notification and send an email: '[Name] mentioned you in a comment on [task title]'."

**Add Email Digest Batching**
> "Claude, instead of sending an email for every task assignment, batch them. If a user receives multiple task assignments within 5 minutes, combine them into a single email: 'You have 3 new task assignments.' This reduces email fatigue."

---

## Troubleshooting

**Emails not arriving**
> "Claude, emails are not showing up. Check: is the RESEND_API_KEY correct? Is the from address valid (use onboarding@resend.dev for testing)? Check the Resend dashboard for delivery logs. On the free tier, you can only send to your signup email."

**Email templates look broken**
> "Claude, the email looks wrong in my inbox. Email rendering varies between clients. Check the templates in the preview route first. Use the React Email components for layout instead of custom CSS. Test in Gmail, Outlook, and Apple Mail if possible."

**Notification count not updating**
> "Claude, the bell badge doesn't update when I get new notifications. Check: is the polling interval set up correctly? Is the API route returning the correct unread count? Check the browser's network tab to verify requests are being made every 30 seconds."

**Notifications sent to wrong users**
> "Claude, a user received a notification meant for someone else. Check the recipientId logic in each notification trigger. Make sure task assignment notifications go to the assignee, not the assigner. Audit each notification creation call."

**Preferences not being respected**
> "Claude, I disabled task assignment emails but I'm still receiving them. Check: is the email service actually checking preferences before sending? Is the preferences query using the correct userId? Add a log line showing the preference check result."

---

## Reflection

Step back and look at what you've built over the last three weeks. TeamTask Pro is a complete SaaS application:

- User authentication with email and OAuth
- Multi-tenant organizations with role-based access
- Projects and tasks with full CRUD
- Member invitations
- Stripe subscription billing with free and paid tiers
- Feature gating based on subscription status
- Transactional email for key events
- In-app notifications with real-time updates
- User notification preferences
- Error monitoring with Sentry
- Product analytics with PostHog
- A marketing landing page
- Deployed and live on Vercel

This is not a tutorial project. This is the same architecture, the same stack, and the same patterns used by real SaaS companies generating real revenue. The difference between TeamTask Pro and a shipped product is polish and users -- not technology.

You've now completed the foundation of the Build phase. You know how to research, plan, and direct Claude Code to build production-quality software. The next projects will push you further -- but the workflow stays the same.
