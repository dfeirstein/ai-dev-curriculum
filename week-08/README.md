# Week 8: Project 2 -- TeamTask Pro Foundation

**Theme:** Direct Claude Code to build a real SaaS application with auth, multi-tenancy, and CRUD.

You built a personal portfolio. Now you're building something bigger -- a multi-user SaaS product. TeamTask Pro is a project management application where teams can collaborate on tasks. It has everything a real SaaS has: user accounts, organizations, role-based access, and a marketing landing page.

This is a multi-week project. This week you build the foundation. Weeks 9 and 10 add payments and notifications. By the end, you'll have a complete, production-ready SaaS application.

---

## Learning Objectives

1. Understand what makes something a **SaaS product** -- multi-user, multi-tenant, subscription-based.
2. Understand **multi-tenancy** -- organizations/workspaces where each team sees only their own data.
3. Understand **data modeling hierarchies** -- users, organizations, projects, tasks, and how they relate.
4. Understand **role-based access control** -- owner, admin, member -- who can do what.
5. Understand **invitation systems** -- how users join an organization.
6. Direct Claude Code to build a **complete SaaS foundation** with auth, organizations, and CRUD.

## What You Ship

**TeamTask Pro** -- a project management SaaS with user authentication, organizations with role-based access, projects, tasks (full CRUD), member invitations, and a marketing landing page. Deployed to Vercel.

## Time Commitment

~15-18 hours across the week.

## Prerequisites

- **Weeks 1-7 completed.** You've built a portfolio and understand the full stack.
- GitHub account, Vercel account, Neon database.
- Accounts for Better Auth (uses your database), Sentry, and PostHog from Week 6.

## This Week's Materials

| File | What It Covers |
|------|---------------|
| [guide.md](guide.md) | Conceptual overview of SaaS, multi-tenancy, data modeling, and roles |
| [project.md](project.md) | Step-by-step project: build TeamTask Pro's foundation |
| [resources.md](resources.md) | Links for deeper exploration |

## How to Approach This Week

1. **Read guide.md** thoroughly. Understanding multi-tenancy and data modeling is critical before you start building.
2. **Think about the data model** before directing Claude Code. Draw it out if that helps. Users belong to organizations. Organizations have projects. Projects have tasks. Roles control permissions.
3. **Work through project.md** using the research, plan, execute workflow.
4. **Test with multiple users.** Create two accounts and verify they see only their own organization's data.

After this week, you'll have the skeleton of a real SaaS product. Weeks 9 and 10 will add the business layer (payments) and communication layer (notifications and email).
