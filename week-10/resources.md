# Week 10 Resources

Reference material for transactional email, in-app notifications, and communication patterns. Focus on understanding the concepts and browsing the tools.

---

## Resend and Email

- [Resend Documentation](https://resend.com/docs/introduction) -- Getting started guide. Simple API, straightforward setup.
- [Resend with Next.js](https://resend.com/docs/send-with-nextjs) -- Specific guide for sending email from Next.js applications.
- [Resend Domain Verification](https://resend.com/docs/dashboard/domains/introduction) -- How to set up SPF and DKIM for custom sending domains.
- [Resend API Reference](https://resend.com/docs/api-reference/emails/send-email) -- The API for sending emails. Simple and well-documented.

## React Email

- [React Email](https://react.email/) -- Official site. Browse the component library and examples.
- [React Email Components](https://react.email/docs/components/html) -- The components available for building email templates (Button, Heading, Text, Section, etc.).
- [React Email Examples](https://demo.react.email/) -- Gallery of example email templates. Browse for design inspiration.
- [React Email with Next.js](https://react.email/docs/integrations/next) -- Integration guide for previewing and sending React Email templates in Next.js.

## Notification Patterns

- [Notification Design Patterns](https://www.nngroup.com/articles/push-notification/) -- Nielsen Norman Group's research on notification design. Understand when notifications help and when they annoy.
- [In-App Notifications Best Practices](https://www.intercom.com/blog/in-app-notifications/) -- Intercom's guide to in-app notification design. Good conceptual overview.
- [Notification Bell Pattern](https://www.uxpin.com/studio/blog/notification-design-patterns/) -- UX patterns for the bell icon, badges, dropdowns, and notification centers.

## Real-Time Patterns (Conceptual)

- [Polling vs WebSockets vs SSE](https://ably.com/blog/websockets-vs-long-polling) -- Comparison of real-time communication patterns. Understand the tradeoffs conceptually.
- [Server-Sent Events (MDN)](https://developer.mozilla.org/en-US/docs/Web/API/Server-sent_events) -- Mozilla's documentation on SSE. Reference for the stretch goal.
- [WebSocket API (MDN)](https://developer.mozilla.org/en-US/docs/Web/API/WebSocket) -- Mozilla's documentation on WebSockets. Conceptual reference.

## Email Deliverability

- [Email Authentication Explained](https://resend.com/blog/email-authentication-explained) -- Resend's guide to SPF, DKIM, and DMARC. Clear, practical explanation.
- [Why Emails Go to Spam](https://www.mailgun.com/blog/email/why-are-my-emails-going-to-spam/) -- Common reasons emails end up in spam folders and how to prevent it.
- [Can I Email](https://www.caniemail.com/) -- Like "Can I Use" but for email. Check which CSS and HTML features are supported across email clients.

## UX and Design

- [shadcn/ui Popover](https://ui.shadcn.com/docs/components/popover) -- The component you'll likely use for the notification dropdown.
- [shadcn/ui Badge](https://ui.shadcn.com/docs/components/badge) -- For the unread count badge on the bell icon.
- [shadcn/ui Switch](https://ui.shadcn.com/docs/components/switch) -- For the notification preference toggles.
- [Lucide Icons](https://lucide.dev/) -- Icon library used by shadcn/ui. Search for "bell" and "mail" icons.

## Scheduling and Background Jobs (Stretch Goals)

- [Vercel Cron Jobs](https://vercel.com/docs/cron-jobs) -- How to run scheduled functions on Vercel. Useful for weekly digest emails.
- [Inngest](https://www.inngest.com/) -- A background job service that works well with Next.js. An alternative for more complex scheduling needs.
