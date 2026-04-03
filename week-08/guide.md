# Week 8 Guide: TeamTask Pro Foundation

Your bookmark manager serves one person. Your portfolio is a personal site. Now you're building something that serves many people, working together, on teams. That shift -- from personal tool to multi-user product -- is what separates a project from a SaaS business.

---

## Part 1: What Makes Something a SaaS

SaaS stands for Software as a Service. It means software that people access through a browser, typically on a subscription basis. Gmail, Slack, Notion, Linear, Asana -- all SaaS products.

Three things define a SaaS product:

**Multi-user.** Many people use the same application. Each person has their own account, their own data, their own experience. The application serves thousands of users simultaneously, but each one feels like it was built just for them.

**Multi-tenant.** Users are organized into tenants -- usually called organizations, workspaces, or teams. A company signs up, creates an organization, and invites their team members. Everything that team does is contained within their organization. Other organizations can't see it.

**Subscription model.** Users pay on a recurring basis -- monthly or annually. Different tiers offer different features. Free tiers attract users. Paid tiers generate revenue.

This week you're building the first two: multi-user and multi-tenant. Week 9 adds the subscription model.

### The SaaS Mental Model

When you're directing Claude Code to build a SaaS, think about your app as serving many teams, not one person. Every feature you add needs to ask: "Which organization does this belong to?" Every database query needs to filter by organization. Every permission check needs to consider the user's role within their organization.

This is the biggest conceptual shift from what you've built so far. Your bookmark manager asked "which user?" TeamTask Pro asks "which user, in which organization, with what role?"

---

## Part 2: Multi-Tenancy

Multi-tenancy means multiple organizations share the same application and database, but each one is isolated -- they can't see each other's data.

### How It Works

Imagine three companies all use TeamTask Pro: Acme Corp, Widget Inc, and StartupXYZ. They all use the same application, running on the same servers, storing data in the same database. But:

- Acme Corp can only see Acme Corp's projects and tasks
- Widget Inc can only see Widget Inc's projects and tasks
- StartupXYZ can only see StartupXYZ's projects and tasks

This isolation is enforced at the data level. Every project, task, and piece of data has an `organizationId` that says which organization it belongs to. Every database query includes a filter: "only return data where `organizationId` matches the current user's organization."

### Why It Matters

If multi-tenancy breaks, Organization A can see Organization B's private data. This is a critical security concern. When directing Claude Code, always verify that data queries are properly scoped to the current organization.

### When Directing Claude Code

> "Claude, every database query that returns projects, tasks, or any organization-scoped data must filter by the current user's organizationId. Never return data from another organization. Add this as a rule in CLAUDE.md."

---

## Part 3: Data Modeling

Data modeling is deciding what things exist in your system and how they relate to each other. For TeamTask Pro, the hierarchy is:

### The Hierarchy

**Users** are at the top. A user is a person with an account -- email, password, profile.

**Organizations** are teams or companies. An organization has a name and settings. Users join organizations. A user can belong to multiple organizations (think of how you might be in a Slack workspace for work and another for a community).

**Memberships** connect users to organizations. A membership says "this user belongs to this organization, with this role." The membership is where the role lives -- you might be an owner of your own organization but just a member of another.

**Projects** belong to an organization. A project groups related tasks together. "Website Redesign," "Q1 Marketing," "Mobile App" -- each is a project.

**Tasks** belong to a project. A task is a single unit of work. It has a title, description, status (todo, in progress, done), priority, and an assignee (the team member responsible).

### Relationships

Think of it as a tree:

```
User
  └── Membership (with role)
        └── Organization
              └── Project
                    └── Task (with assignee)
```

A user has many memberships. An organization has many members, many projects. A project has many tasks. A task has one assignee (who must be a member of the organization).

### Why This Matters for Directing Claude Code

When you describe this data model to Claude Code, be explicit about the relationships and constraints. "Tasks belong to projects. Projects belong to organizations. A task's assignee must be a member of the same organization." These constraints prevent bugs where a user from Organization A gets assigned a task in Organization B.

### When Directing Claude Code

> "Claude, design the database schema for TeamTask Pro. The hierarchy is: users -> memberships -> organizations -> projects -> tasks. Each membership has a role (owner, admin, member). Tasks have a title, description, status (todo, in_progress, done), priority (low, medium, high, urgent), and an assignee who must be a member of the organization. Use Drizzle ORM with proper foreign keys and constraints."

---

## Part 4: Role-Based Access Control

Not every team member should have the same permissions. The person who created the organization should be able to delete it. A regular member should not.

### The Roles

**Owner.** The person who created the organization. Full control: can change settings, manage billing, delete the organization, manage all members. There should always be at least one owner.

**Admin.** A trusted team member. Can manage projects, manage tasks, and invite new members. Cannot delete the organization or manage billing.

**Member.** A regular team member. Can create and manage their own tasks, view projects they're assigned to, and update task statuses. Cannot invite members or change organization settings.

### How It Works

When a user performs an action, your app checks: "Does this user's role in this organization allow this action?" If yes, proceed. If no, show an error.

This check happens in two places:

1. **The UI.** Don't show buttons the user can't use. If a member can't invite people, don't show them the invite button. This is for user experience.
2. **The API.** Even if someone manipulates the UI, the API must enforce permissions. This is for security. Never trust the frontend alone.

### When Directing Claude Code

> "Claude, implement role-based access control. Owners can do everything. Admins can manage projects, tasks, and invite members. Members can create and update tasks and view projects. Check permissions in both the UI (hide unauthorized actions) and the API (reject unauthorized requests). Define the permission matrix in CLAUDE.md."

---

## Part 5: The Invitation System

Teams grow. When a new person joins the company, they need to join the organization in TeamTask Pro. That's the invitation system.

### How Invitations Work

1. An admin or owner enters an email address and selects a role
2. The app creates an invitation record (email, role, organization, status: pending)
3. An email is sent to the invited person with a link
4. The person clicks the link, creates an account (or logs in if they already have one), and joins the organization with the assigned role
5. The invitation status changes to accepted

### Edge Cases to Think About

- What if the person is already a member? Don't send a duplicate invitation.
- What if the invitation expires? Set a reasonable expiration (7 days is common).
- What if the person doesn't have an account yet? The invitation flow should handle both new and existing users.
- Can an invitation be revoked? Yes -- the inviter should be able to cancel a pending invitation.

### When Directing Claude Code

> "Claude, build an invitation system. Admins and owners can invite users by email with a selected role. Create an invitations table tracking email, role, organization, status (pending/accepted/expired/revoked), and expiration date. Handle both new users and existing users. Show pending invitations in the organization settings."

---

## Part 6: The Landing Page

Every SaaS needs a public-facing page that explains what it does. This is separate from the authenticated app -- it's the page people see before they sign up.

### What a SaaS Landing Page Includes

- **Hero section.** What the product does in one sentence. A screenshot or demo.
- **Features.** Three to five key capabilities, briefly explained.
- **Pricing.** What it costs. (You'll add real pricing in Week 9 -- for now, placeholder tiers.)
- **Social proof.** Testimonials, user counts, logos. (For now, you can skip this or use placeholder content.)
- **Call to action.** A "Get Started" button that leads to signup.

The landing page is a marketing tool. Its job is to convince someone to try the product. Keep it clear, focused, and honest about what the product does.

### When Directing Claude Code

> "Claude, build a marketing landing page for TeamTask Pro at the root route /. Include a hero section, feature highlights, placeholder pricing tiers (Free and Pro), and a call-to-action button that links to signup. Logged-in users should be redirected to the dashboard instead."

---

## What's Next

Head to [project.md](project.md) to build TeamTask Pro.
