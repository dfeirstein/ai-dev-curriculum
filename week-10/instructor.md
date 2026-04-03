# Week 10: TeamTask Pro — Notifications — Instructor Guide

## Week at a Glance
- Theme and key concepts: Building a notification system that handles both email (Resend) and in-app notifications (bell icon with unread count). Students learn notification architecture: a central service that receives events and routes them to the right channels based on user preferences.
- What students ship: TeamTask Pro with transactional emails (welcome, invitation, task assignment), in-app notification bell with unread count, and user notification preferences — deployed live
- Estimated hours: 10-12
- Difficulty: 3

## Pacing Guide
- **Day 1-2:** Read guide.md. Set up Resend account and verify a sender email. Research notification architecture with Claude — central event handler, channel routing, preference checks. Design the notifications database table. Build the welcome email with React Email template.
- **Day 3-4:** Build the in-app notification system: notifications table, API routes for fetching/marking-read, bell icon UI with unread count. Wire up events: task assigned, member invited, task status changed. Build notification preferences page.
- **Day 5:** Polish email templates. Test all notification triggers end-to-end. Verify preferences are respected (if a user turns off email for task assignments, they should still get in-app but not email). Deploy.

## Getting Students Started
- Kick off by asking: "When someone assigns you a task in Asana or Linear, what happens?" They get an email and a notification in the app. Two channels, same event. That is what they are building this week — a system that takes an event and decides how to notify the user.
- Common confusion: students think email and in-app notifications are separate features. Reframe: they are two channels for one notification system. The event happens once ("task assigned"). The notification service decides: send an email? Show an in-app notification? Both? Neither? That depends on the user's preferences.
- Students should have open: Ghostty terminal, browser with the app, Resend dashboard, and a scratch area for mapping out which events trigger which notifications.

## Check-in Points
- **After guide.md:** Can the student list the notification events they plan to support? (Welcome email on signup, invitation email, task assignment notification, etc.) Can they explain the difference between transactional email (triggered by an action) and marketing email (sent in bulk)? They are building transactional email only.
- **Mid-project checkpoint:** At least one email sends successfully (welcome email on signup is the easiest to test). The notifications database table exists and can store in-app notifications. If neither email nor in-app notifications is working by mid-week, focus on getting one channel working end-to-end before adding the second.
- **Pre-ship review:** Full flow: sign up (welcome email arrives), get invited to an org (invitation email arrives), get assigned a task (in-app notification appears, bell shows unread count, clicking marks it read). Preferences page works — turning off email notifications for task assignments stops those emails but not the in-app notification.

## Common Pitfalls
- **Email deliverability on free tier.** Resend's free tier restricts sending to verified email addresses or emails on your verified domain. Students will try to send to a random address and wonder why it never arrives. For testing, have them send to the email address they signed up with on Resend, or verify a test recipient.
- **Notification architecture decisions.** Students may create separate, disconnected functions for each notification type instead of a central notification service. Push them toward a pattern: `notify({ event: 'task_assigned', userId, data })` that routes to the appropriate channels. This is cleaner and makes preferences possible.
- **In-app notification polling frequency.** Students who implement polling may poll every second, creating unnecessary load. 30-second polling intervals are fine for this use case. Alternatively, they can refetch on page navigation. Real-time (WebSockets) is a stretch goal, not a requirement.
- **Forgetting to handle the "read" state.** Student builds the notification list but every notification always shows as unread. They need a `readAt` timestamp column and an API route to mark notifications as read (individually or "mark all read").
- **React Email template complexity.** Students may try to build elaborate HTML email templates. Email HTML is notoriously painful — most CSS doesn't work, layouts break across clients. Direct them to use React Email's pre-built components (Section, Row, Column, Button, Text) which handle cross-client compatibility.

## How to Know the Student Gets It
- They can explain the notification flow: event occurs -> notification service receives it -> checks user preferences -> sends to appropriate channels (email, in-app, or both). This is an architecture understanding, not just "I got email working."
- They test notifications from the recipient's perspective, not just the sender's. They log in as the user who was assigned a task and verify the notification appears. They check their inbox for the email.
- Their preference system actually works — disabling a notification channel for a specific event type stops that channel without affecting others.
- Red flags: Notifications are hardcoded (always send both email and in-app, no preferences). The bell icon exists but clicking it does nothing. Welcome email works but no other notification type is implemented. Student only tested with one user account.

## Support Strategies
- If stuck on email setup: Walk through Resend's setup checklist: API key in environment variables, sender email verified or domain verified, Resend SDK installed. Have them send a test email using the Resend dashboard first (not through their app) to confirm the account works. Then move to programmatic sending.
- If rushing without reviewing: Ask them to turn off email notifications for task assignments in their preferences, then assign themselves a task. Did the email still arrive? If yes, preferences are not actually connected. This is the test that separates working features from demo features.
- If frustrated: This is Week 10 and students have built a lot. Fatigue is real. Remind them that this is the final feature week for TeamTask Pro — after this, the product is complete. The finish line is visible. Also, compared to Week 8 (multi-tenancy) and Week 9 (payments), notifications are architecturally simpler.

## Differentiation
- **Moving fast?** Add digest emails (daily summary of notifications instead of one email per event). Add notification categories with separate preference controls. Add real-time notifications using WebSockets or server-sent events. Add email unsubscribe links that update preferences.
- **Struggling?** Focus on just two notifications: welcome email on signup and in-app notification on task assignment. Skip preferences entirely — always send both channels. Two working notifications are better than a half-built notification system.
- **Minimum viable ship:** At least one transactional email sends successfully (welcome email on signup). In-app notification bell shows at least one type of notification with an unread count. Marking notifications as read works. Deployed live. TeamTask Pro is now a complete SaaS product.
