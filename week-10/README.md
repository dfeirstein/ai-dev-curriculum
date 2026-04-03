# Week 10: TeamTask Pro -- Notifications and Email

**Theme:** Direct Claude Code to add transactional email and in-app notifications to your SaaS.

TeamTask Pro has users, organizations, payments, and features. But it's silent. When someone assigns you a task, you don't know unless you're staring at the app. When a new member joins, there's no welcome message. When an invitation is sent, there's no email.

This week you add the communication layer: transactional email through Resend and in-app notifications with the familiar bell icon pattern. By the end, TeamTask Pro will proactively tell users what's happening -- the hallmark of a polished, production-quality SaaS.

---

## Learning Objectives

1. Understand **transactional email** -- what it is, when to send it, and how Resend handles delivery.
2. Understand **email templates** conceptually -- React Email builds emails with components.
3. Understand **in-app notifications** -- the bell icon pattern, unread counts, and notification storage.
4. Understand **real-time vs polling** -- how to show new notifications without refreshing.
5. Understand **notification architecture** -- what events trigger what notifications.
6. Understand **email deliverability basics** -- SPF, DKIM, and why Resend handles them for you.

## What You Ship

**TeamTask Pro with notifications and email** -- Welcome emails, invitation emails, task assignment notifications (email + in-app), a notification bell with unread count, and user notification preferences.

## Time Commitment

~12-15 hours across the week.

## Prerequisites

- **Week 9 completed.** TeamTask Pro has auth, organizations, payments, and feature gating.
- A [Resend account](https://resend.com) (free tier is sufficient).
- Your TeamTask Pro repo deployed on Vercel.

## This Week's Materials

| File | What It Covers |
|------|---------------|
| [guide.md](guide.md) | Conceptual overview of transactional email, in-app notifications, and real-time patterns |
| [project.md](project.md) | Step-by-step project: add notifications and email to TeamTask Pro |
| [resources.md](resources.md) | Links for deeper exploration |

## How to Approach This Week

1. **Read guide.md** thoroughly. Notifications seem simple, but the architecture decisions matter: what events trigger which notifications, through which channels, and with what user controls.
2. **Map out your notification events** before building. Write down every event that should notify someone and how.
3. **Work through project.md** using the research, plan, execute workflow.
4. **Test with real email.** Send real emails to yourself (Resend's free tier allows this). Read them. Do they make sense? Would you want to receive them?

After this week, TeamTask Pro is complete. It has everything a production SaaS needs: auth, organizations, payments, email, and notifications. You've built a real product.
