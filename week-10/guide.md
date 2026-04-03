# Week 10 Guide: Notifications and Email

A great application doesn't wait for you to come back and check it. It tells you when something needs your attention. That's the communication layer: emails that arrive in your inbox, notifications that appear in the app, and the intelligence to know when each one is appropriate.

---

## Part 1: Transactional Email

### What Transactional Email Is

Transactional emails are triggered by specific events, not marketing campaigns. Every time something happens that a user should know about, the app sends an email automatically.

Common transactional emails in a SaaS:

- **Welcome email.** Sent when a user signs up. Confirms their account and gets them started.
- **Invitation email.** Sent when someone is invited to join an organization. Contains a link to accept.
- **Task assignment.** Sent when a task is assigned to you. Tells you what was assigned and by whom.
- **Password reset.** Sent when a user requests to reset their password. Contains a secure link.
- **Payment receipt.** Sent when a subscription payment succeeds. Confirms the charge.
- **Payment failed.** Sent when a payment fails. Prompts the user to update their payment method.
- **Weekly digest.** A periodic summary of activity in the user's organizations.

The key insight: transactional emails are not spam. They're messages the user expects because they took an action or something happened that affects them. A welcome email is expected after signup. A task notification is expected after assignment. Users want these emails.

### Why Resend

Sending email from an application is deceptively complex. You can't just call a "send email" function and expect it to arrive. Emails go through spam filters, reputation checks, authentication protocols, and delivery infrastructure. Getting all of this right is a full-time job.

Resend handles delivery infrastructure so your emails actually arrive. It provides a simple API: you call it with a recipient, subject, and body, and Resend handles everything else -- delivery, tracking, bounce handling, and compliance.

### React Email

Resend works with React Email, a library that lets you build email templates using React components. This is significant because email HTML is notoriously difficult -- it uses decades-old rendering engines that don't support modern CSS. React Email abstracts this away: you write components, and it generates the compatible HTML.

You don't need to understand React Email's internals. You just need to know that Claude Code can build professional email templates with it, using the same component approach you use for web pages.

### When Directing Claude Code

> "Claude, set up Resend for transactional email. Install the Resend SDK and React Email. Create a reusable email sending utility. Build email templates for: welcome (on signup), invitation (when invited to an org), and task assignment (when assigned a task)."

---

## Part 2: Email Templates

### What Makes a Good Transactional Email

**Clear subject line.** The user should know what the email is about before opening it. "You've been assigned a task: Fix login bug" is better than "New notification."

**One purpose per email.** Each email should do one thing. The welcome email welcomes. The invitation email invites. Don't combine multiple messages into one email.

**Clear call to action.** Most transactional emails want the user to do something: click a link, view a task, accept an invitation. Make that action obvious with a prominent button.

**Brief content.** Transactional emails should be scannable in seconds. Subject line, one or two sentences of context, a button, done.

**Consistent branding.** Use the same colors, logo, and typography as your app. The email should feel like it came from TeamTask Pro, not a generic system.

### The Template Structure

A typical transactional email template has:

1. **Header** -- logo and app name
2. **Greeting** -- "Hi [name],"
3. **Body** -- one or two sentences explaining what happened
4. **Action** -- a button linking to the relevant page in the app
5. **Footer** -- unsubscribe link, company info, support contact

### When Directing Claude Code

> "Claude, create React Email templates for TeamTask Pro. Each template should have a consistent header with the TeamTask Pro logo/name, a greeting, a brief message, a CTA button, and a footer. The design should match the app's color scheme. Build templates for: welcome, invitation, task-assigned, and payment-failed."

---

## Part 3: In-App Notifications

### The Bell Icon Pattern

You've seen this in every SaaS product: a bell icon in the navigation bar with a badge showing the number of unread notifications. Click it, and a dropdown shows recent notifications. Click a notification, and you're taken to the relevant page.

This pattern is universal because it works. Users understand it instantly. It provides timely information without being intrusive.

### How In-App Notifications Work

The architecture is straightforward:

1. **Something happens** that a user should know about (task assigned, member joined, comment added)
2. **A notification record is created** in the database (recipient, message, link, read/unread, timestamp)
3. **The UI shows the notification** -- the bell badge increments, the dropdown shows the new item
4. **The user reads the notification** -- clicking it marks it as read and navigates to the relevant page

### Notification Data Model

A notification needs:
- **recipientId** -- who should see this notification
- **type** -- what kind of notification (task_assigned, member_joined, comment_added)
- **title** -- brief description ("New task assigned")
- **message** -- the details ("Sarah assigned you 'Fix login bug' in Project Alpha")
- **link** -- where to go when clicked (/org/acme/projects/alpha/tasks/123)
- **read** -- boolean, whether the user has seen it
- **createdAt** -- when it was created

### When Directing Claude Code

> "Claude, build an in-app notification system. Create a notifications table (recipientId, type, title, message, link, read, createdAt). Add a bell icon to the nav bar with an unread count badge. Add a dropdown that shows recent notifications. Clicking a notification marks it as read and navigates to the link. Add a 'Mark all as read' button."

---

## Part 4: Real-Time vs Polling

When a new notification is created, how does the user's browser know about it without refreshing the page?

### Polling

The simplest approach: the browser asks the server "any new notifications?" on a regular interval -- say, every 30 seconds. This is called polling.

Pros: simple to implement, works everywhere.
Cons: wastes requests when nothing has changed, slight delay before new notifications appear.

For TeamTask Pro, polling is the right choice. It's simple, reliable, and the delay is negligible for a project management tool.

### Real-Time (Conceptual)

For comparison, real-time approaches push notifications to the browser instantly:

**WebSockets** maintain a persistent connection between the browser and server. The server can push data to the client at any time. Used in chat applications where milliseconds matter.

**Server-Sent Events (SSE)** are a lighter-weight alternative. The server keeps a connection open and sends events when they happen.

These are more complex to implement and to scale. For a project management tool, polling every 30 seconds is perfectly adequate. If you built a chat application, you'd need real-time.

### When Directing Claude Code

> "Claude, use polling to check for new notifications. Every 30 seconds, fetch the unread notification count and update the bell badge. When the user opens the notification dropdown, fetch the recent notifications. Keep it simple -- no WebSockets needed for this use case."

---

## Part 5: Notification Architecture

Before building, decide what events trigger what notifications. This is a design decision, not a technical one.

### The Notification Map

For TeamTask Pro:

| Event | Email | In-App | Recipient |
|-------|-------|--------|-----------|
| User signs up | Welcome email | -- | The new user |
| Invited to org | Invitation email | -- | The invited person |
| Invitation accepted | -- | "X joined your org" | Org admins and owner |
| Task assigned | Assignment email | "X assigned you a task" | The assignee |
| Task status changed | -- | "Task moved to Done" | The task creator |
| Task due soon | Reminder email | "Task due tomorrow" | The assignee |
| Payment failed | Payment failed email | "Payment issue" | Org owner |

Not every event needs both email and in-app notification. Some events (like invitation accepted) only need an in-app notification because the recipient is likely already in the app. Others (like invitation sent) only need email because the recipient might not have an account yet.

### When Directing Claude Code

> "Claude, here's the notification map for TeamTask Pro: [paste the table above]. Implement a notification service that takes an event type and creates the appropriate notifications (email, in-app, or both) based on this map. This centralizes the notification logic so we don't scatter it across the codebase."

---

## Part 6: Email Deliverability

You need to understand deliverability at a high level, even though Resend handles the technical details.

### Why Emails End Up in Spam

Email providers (Gmail, Outlook, etc.) aggressively filter spam. To determine if an email is legitimate, they check:

**SPF (Sender Policy Framework).** A DNS record that says "these servers are allowed to send email on behalf of this domain." Without SPF, anyone could send email pretending to be from your domain.

**DKIM (DomainKeys Identified Mail).** A cryptographic signature on each email that proves it wasn't tampered with in transit. The recipient's email provider verifies the signature against a public key in your DNS records.

**DMARC.** A policy that tells email providers what to do if SPF or DKIM checks fail (reject, quarantine, or accept).

### What Resend Handles

When you use Resend, they handle SPF and DKIM configuration for their sending domain. If you use a custom "from" address (like `notifications@yourdomain.com`), you'll need to add DNS records that Resend provides. This is a one-time setup.

The important thing to know: if your emails aren't arriving, the first thing to check is DNS configuration. Resend's dashboard shows you the status of your domain verification.

### Notification Preferences

Users should be able to control what notifications they receive. This is both a good practice and a legal requirement in many jurisdictions.

A simple preferences system:
- Email notifications: on/off for each type (task assignments, reminders, digests)
- In-app notifications: generally always on (the user can ignore them)
- Unsubscribe links in every email

### When Directing Claude Code

> "Claude, add a notification preferences page in user settings. Let users toggle email notifications for each type: task assignments, task reminders, weekly digest, and payment alerts. Respect these preferences when sending emails -- check the user's preferences before sending. Include an unsubscribe link in every email footer."

---

## What's Next

Head to [project.md](project.md) to add notifications and email to TeamTask Pro.
