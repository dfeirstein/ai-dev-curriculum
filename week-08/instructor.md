# Week 8: TeamTask Pro Foundation — Instructor Guide

## Week at a Glance
- Theme and key concepts: Building a real SaaS product from scratch. Multi-tenancy (organizations), role-based access control (owner/admin/member), and the data model complexity that comes with a multi-user, multi-team application. This is the hardest week in the curriculum.
- What students ship: TeamTask Pro with auth, organizations, role-based access, projects, tasks, member invitations, and a marketing landing page — deployed live
- Estimated hours: 12-15
- Difficulty: 5

## Pacing Guide
- **Day 1-2:** Read guide.md thoroughly — do not skim. Research multi-tenancy and RBAC with Claude. Design the full data model: users, organizations, memberships (with roles), projects, tasks. Set up the project scaffold, configure Better Auth with organization support, run initial migrations.
- **Day 3-4:** Build the core flows: create organization, invite members, create projects within an org, create and manage tasks within projects. Implement RBAC — owners can do everything, admins can manage members and projects, members can only manage tasks. Build the dashboard layout.
- **Day 5:** Marketing landing page. Edge case testing (what happens when a non-member tries to access an org? what happens when the last owner tries to leave?). Polish and deploy.

## Getting Students Started
- Kick off by showing them a product they know (Linear, Asana, Notion) and asking: "What happens when you create a team workspace? What can an admin do that a regular member can't?" This grounds the abstract concepts of multi-tenancy and RBAC in something they already understand.
- Common confusion: students do not understand what "multi-tenancy" means. Explain it concretely: when User A logs into their company's TeamTask, they see their company's projects. When User B logs into their company's TeamTask, they see totally different projects. Same app, different data, separated by organization. That's multi-tenancy.
- Students should have open: Ghostty terminal, browser, Neon dashboard, and a sketch (even on paper) of the data model relationships.

## Check-in Points
- **After guide.md:** Can the student draw the data model on paper? Users -> Memberships -> Organizations -> Projects -> Tasks. Can they explain what a membership record contains (userId, orgId, role)? Can they explain why "role" lives on the membership, not on the user? (Because a user can be an owner in one org and a member in another.)
- **Mid-project checkpoint:** Auth works. At least one organization can be created. The student can create a project inside the organization and a task inside the project. If the data model is not settled and migrated by mid-week, they are seriously behind — this is a week where falling behind compounds quickly.
- **Pre-ship review:** Test with two users and two organizations. Verify data isolation (Org A's projects don't appear in Org B). Verify role enforcement (a member cannot delete a project, an admin can). Invitation flow works. The marketing landing page exists and links to signup.

## Common Pitfalls
- **Multi-tenancy is conceptually hard.** Students who breezed through earlier weeks may hit a wall here. The data model has real relationships — foreign keys, join tables, cascading deletes. If a student is confused about why a "membership" table exists between users and organizations, stop and draw the entity-relationship diagram. Do not proceed until they can explain it.
- **Role-based access edge cases.** Student implements basic role checks but misses critical edges: Can a member invite other members? (Probably not.) Can an admin remove an owner? (Definitely not.) Can the last owner leave the organization? (What happens to the org?) Map out the permission matrix explicitly before building.
- **Scope overwhelm.** This is a 12-15 hour week with many moving parts. Students who try to build everything simultaneously will get lost. Enforce a strict build order: auth first, then organizations, then memberships/roles, then projects, then tasks, then invitations, then the marketing page. Each layer depends on the one before it.
- **Confusing app pages with marketing pages.** The marketing landing page (public, sells the product) is different from the app dashboard (private, behind auth). Students sometimes try to put them in the same layout. Separate them: marketing pages at `/`, app pages at `/dashboard` or `/app`.
- **Database schema changes without migrations.** This week has the most complex schema yet. Every time Claude modifies the Drizzle schema, the migration must be pushed. Students will see "relation does not exist" errors and panic. Train them: schema change = migration push, every time.

## How to Know the Student Gets It
- They can explain the data model without looking at the code. "A user has memberships. Each membership connects them to an organization with a role. An organization has projects. A project has tasks." If they can trace the relationships, they understand the architecture.
- When they encounter a permission question ("Can a member rename the organization?"), they know where to look — the role on the membership record — and can reason about whether it should be allowed.
- They test with multiple users. Not just "my app works when I use it" but "my app correctly separates data between two different users in two different organizations."
- Red flags: Student has only tested with one user in one organization. Student cannot explain what the membership table does. The marketing page and the app dashboard are the same page. No role-based access control exists — every user can do everything.

## Support Strategies
- If stuck on multi-tenancy: Build it in layers. First, just get organizations working (create, view). Then add memberships (join an org). Then add roles (owner vs member). Then add projects and tasks. Each layer is a small, testable increment.
- If rushing without reviewing: Ask them to log in as a "member" role and try to delete a project. If it works, their RBAC is broken. This single test reveals whether access control is real or cosmetic.
- If frustrated: Acknowledge this is the peak difficulty week. Remind them that Weeks 9 and 10 build on this foundation, so getting it right now pays off. If they need to, it is better to ship a working subset (auth + orgs + projects, no role enforcement) than a broken whole.
- For complex multi-step builds: Have them run `/powerup` lessons "Multiply yourself" (subagents) and "Run tasks in background" to learn how to parallelize work with Claude — e.g., schema migrations running in one agent while UI scaffolding happens in another.

## Differentiation
- **Moving fast?** Add task assignment to specific org members. Add task due dates with overdue highlighting. Add organization settings page. Add an activity feed showing recent actions within the org.
- **Struggling?** Drop member invitations and simplify roles to just "owner" and "member" (skip "admin"). Reduce to organizations and tasks only — skip the "project" layer so tasks belong directly to the org. This still teaches multi-tenancy and roles with less relational complexity.
- **Minimum viable ship:** Auth works. Organizations exist and data is isolated between them. At least two roles exist with different permissions. Projects and tasks can be created within an org. Deployed to a live URL. Marketing landing page exists (even if basic).
