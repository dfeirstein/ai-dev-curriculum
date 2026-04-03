# Project: TeamTask Pro -- Foundation

**What you're building:** A project management SaaS with user authentication, organizations with role-based access, projects, tasks, member invitations, and a marketing landing page.

**Time estimate:** 12-15 hours

**What you'll need open:** Ghostty (your terminal), a web browser, and your Neon database dashboard.

---

## The Brief

You're building a real SaaS product. Not a toy, not a demo -- a product that teams could actually use to manage projects and tasks. This week is the foundation: accounts, organizations, data, and permissions. Weeks 9 and 10 add payments and notifications.

Think of this as building Linear, Asana, or Trello. Simpler, but the same architecture.

---

## Phase 1: Research

Start Claude Code and explore the problem space:

> "Claude, I'm building a project management SaaS called TeamTask Pro. It needs multi-tenant organizations, role-based access (owner/admin/member), projects within organizations, and tasks within projects. What's the best data model for this? How should I structure the database relationships?"

> "Claude, how does Better Auth handle organization/team features? Does it have built-in support for organizations and roles, or do I need to build that on top of the auth layer?"

> "Claude, what are the most important CRUD operations for a project management tool? What status workflows do tasks typically have?"

> "Claude, how should I handle multi-tenancy in Next.js? Should organizations be in the URL path (like /org/acme/projects) or determined from the session?"

---

## Phase 2: Plan

> "Claude, plan the full build for TeamTask Pro. Here's the spec:
>
> **Auth:**
> - Better Auth with email/password and GitHub OAuth
> - Session management, protected routes
>
> **Organizations:**
> - Users create or join organizations
> - Each organization is isolated (multi-tenant)
> - Roles: owner, admin, member
> - Organization settings page
>
> **Projects:**
> - Belong to an organization
> - Have a name, description, and status (active/archived)
> - Listed on the organization dashboard
>
> **Tasks:**
> - Belong to a project
> - Have title, description, status (todo/in_progress/done), priority (low/medium/high/urgent), assignee, due date
> - Full CRUD: create, read, update, delete
> - Can be assigned to any organization member
>
> **Invitations:**
> - Admins and owners can invite by email
> - Invited users receive an email and join with the assigned role
>
> **Landing page:**
> - Marketing page at / with features, placeholder pricing, and signup CTA
>
> **Monitoring:**
> - Sentry for errors, PostHog for analytics
>
> Plan the database schema, API routes, page structure, and component breakdown. Use Next.js, TypeScript, Tailwind, shadcn/ui, Drizzle ORM, Postgres on Neon, Zod for validation."

Review the plan carefully. Key questions:

- Does the database schema enforce multi-tenancy? (Every org-scoped table has an organizationId)
- Are roles defined clearly? What can each role do?
- How does the invitation flow work for new users vs. existing users?
- What does the URL structure look like?
- Are there proper foreign key constraints?

---

## Phase 3: Execute

### Step 1: Project Setup

> "Claude, create the Next.js project for TeamTask Pro with TypeScript, Tailwind, shadcn/ui, and Drizzle ORM configured for Neon Postgres. Set up the CLAUDE.md with project conventions, including the multi-tenancy rule: every database query for org-scoped data must filter by organizationId."

> "Claude, set up the environment variables: DATABASE_URL, BETTER_AUTH_SECRET, GITHUB_CLIENT_ID, GITHUB_CLIENT_SECRET, SENTRY_DSN, NEXT_PUBLIC_POSTHOG_KEY. Create .env.local and .env.example."

### Step 2: Database Schema

> "Claude, create the full database schema with Drizzle ORM:
> - users (managed by Better Auth)
> - organizations (id, name, slug, createdAt)
> - memberships (userId, organizationId, role: owner/admin/member, joinedAt)
> - projects (id, organizationId, name, description, status: active/archived, createdAt)
> - tasks (id, projectId, title, description, status: todo/in_progress/done, priority: low/medium/high/urgent, assigneeId, dueDate, createdAt, updatedAt)
> - invitations (id, organizationId, email, role, status: pending/accepted/expired/revoked, invitedById, expiresAt, createdAt)
>
> Add proper foreign keys, indexes, and constraints. Run the migration."

Verify the database:

> "Claude, show me the migration SQL that was generated. I want to verify the foreign keys and constraints are correct."

### Step 3: Authentication

> "Claude, integrate Better Auth with email/password and GitHub OAuth. Create login and signup pages with shadcn/ui. After signup, redirect to an organization creation flow -- new users need to either create an organization or accept a pending invitation."

Test thoroughly:
- Sign up with email and password
- Log out and log back in
- Sign up with a second account (use a different email)

### Step 4: Organization Management

> "Claude, build the organization system:
> 1. After first login, show a 'Create Organization' flow if the user has no organizations
> 2. Organization creation form (name, slug)
> 3. The creator automatically becomes the owner
> 4. Organization dashboard showing projects and team members
> 5. Organization settings page (name, slug -- owner only)
> 6. Organization switcher in the nav for users who belong to multiple organizations
> All data queries must be scoped to the current organization."

Test:
- Create an organization
- Verify you're listed as owner
- Check that the dashboard is empty (no projects yet)

### Step 5: Role-Based Access Control

> "Claude, implement role-based access control:
> - Owner: all permissions (settings, billing, invite, manage members, manage projects/tasks, delete org)
> - Admin: invite members, manage projects/tasks, view settings
> - Member: create/update own tasks, view projects, update task status
>
> Enforce permissions in both the UI (hide unauthorized buttons/pages) and the API (reject unauthorized requests with 403). Create a permissions utility that checks roles."

### Step 6: Projects

> "Claude, build the projects feature:
> 1. Projects list page within an organization
> 2. Create project form (name, description) -- admin and owner only
> 3. Project detail page showing tasks
> 4. Archive/unarchive project -- admin and owner only
> 5. Edit project details -- admin and owner only
> Use shadcn/ui components. Validate all inputs with Zod."

Test:
- Create a project
- Edit the project name
- Verify it shows on the organization dashboard

### Step 7: Tasks

> "Claude, build the tasks feature:
> 1. Task list within a project, with filters for status and priority
> 2. Create task form (title, description, status, priority, assignee, due date)
> 3. Task detail view/edit
> 4. Drag-and-drop or click-to-change status columns (todo, in progress, done) -- a Kanban-style board
> 5. Assign tasks to any organization member
> 6. Delete task (with confirmation)
> Validate all inputs with Zod. The assignee dropdown should only show members of the current organization."

Test:
- Create several tasks with different statuses and priorities
- Assign a task
- Move tasks between status columns
- Filter by status and priority

### Step 8: File Attachments with Vercel Blob

> "Claude, add file attachment support to tasks using Vercel Blob for storage:
> 1. Users can attach files (images, PDFs, documents) to any task
> 2. Upload files to Vercel Blob and store the returned URL in the database
> 3. Display attached files on the task detail view with thumbnails for images
> 4. Allow deleting attachments
> 5. Add a task_attachments table (id, taskId, fileName, fileUrl, fileSize, mimeType, uploadedById, createdAt)
> 6. Limit file size to 10MB
> Validate file types with Zod. Use shadcn/ui for the upload interface."

Test:
- Upload an image to a task — verify it displays as a thumbnail
- Upload a PDF — verify it shows with the filename and a download link
- Delete an attachment — verify it's removed

### Step 9: Invitation System


> "Claude, build the invitation system:
> 1. Invite page accessible to admins and owners
> 2. Form to enter email and select role
> 3. Create invitation record with 7-day expiration
> 4. For now, generate an invitation link (we'll add email in Week 10)
> 5. Invitation acceptance page: if logged in, join the org; if not, redirect to signup then join
> 6. Show pending invitations in organization settings
> 7. Allow revoking pending invitations
> Handle edge cases: already a member, expired invitation, revoked invitation."

Test with your second account:
- From Account A (owner), create an invitation
- Copy the invitation link
- Open it in a different browser / incognito with Account B
- Verify Account B joins the organization with the correct role
- Verify Account B can see the organization's projects and tasks

### Step 10: Landing Page

> "Claude, build a marketing landing page at /. Include:
> - Hero: 'Project management for modern teams' with a screenshot of the app
> - Features section: task management, team collaboration, role-based access
> - Pricing section: Free tier (1 project, 3 members) and Pro tier ($12/month -- unlimited projects and members) -- these are placeholder for now
> - CTA button linking to signup
> - If the user is already logged in, redirect to their organization dashboard
> Make it look professional. Use shadcn/ui components and good typography."

### Step 11: Monitoring

> "Claude, add Sentry for error monitoring and PostHog for analytics. Track these events: user-signed-up, organization-created, project-created, task-created, task-status-changed, member-invited, invitation-accepted."

### Step 12: Quality Gates

> "Claude, run npm run lint and fix any issues."

> "Claude, run npm run typecheck and fix any issues."

> "Claude, run npm run build and make sure it completes without errors."

### Step 13: Deploy

> "Claude, commit all changes, push to GitHub, and deploy to Vercel. Walk me through setting up the environment variables in Vercel."

Test on the live URL:
- Full signup and login flow
- Create organization, projects, tasks
- Invite a second user
- Verify multi-tenancy (each org sees only its data)
- Check Sentry and PostHog dashboards

---

## Acceptance Criteria

Your project is complete when all of these are true:

- [ ] App is live on Vercel
- [ ] Users can sign up with email/password and GitHub OAuth
- [ ] Users can create organizations
- [ ] Organizations are fully isolated (multi-tenant)
- [ ] Role-based access works (owner/admin/member permissions)
- [ ] Projects can be created, edited, and archived within organizations
- [ ] Tasks have full CRUD with status, priority, assignee, and due date
- [ ] Task board shows tasks organized by status
- [ ] Invitation system works (invite by email, accept, join with correct role)
- [ ] Landing page explains the product with pricing placeholders
- [ ] Sentry and PostHog are integrated
- [ ] All inputs validated with Zod
- [ ] `npm run lint` passes
- [ ] `npm run typecheck` passes
- [ ] `npm run build` completes without errors
- [ ] Code is on GitHub

---

## Stretch Goals

**Add Task Comments**
> "Claude, add a comment system to tasks. Organization members can leave comments on tasks. Show comments in chronological order on the task detail page. Validate comment input with Zod."

**Add Task Labels/Tags**
> "Claude, add custom labels to tasks. Organization admins can create labels (name + color). Any member can apply labels to tasks. Add label filtering to the task list."

**Add a Dashboard with Metrics**
> "Claude, add an organization dashboard that shows: total tasks by status (pie chart), tasks due this week, recently completed tasks, and a team activity feed. Use a charting library like Recharts."

**Add Search**
> "Claude, add a global search that searches across projects and tasks within the current organization. Use a command palette (Cmd+K) pattern with shadcn/ui."

---

## Troubleshooting

**Multi-tenancy leak -- seeing other org's data**
> "Claude, I can see data from another organization. Audit every database query and API route to ensure they filter by the current user's organizationId. This is a critical security issue."

**Role permissions not enforcing**
> "Claude, a member can access admin-only features. Check the permission checks in both the API routes and the UI components. Make sure the API rejects unauthorized requests even if the UI is bypassed."

**Invitation link not working**
> "Claude, the invitation acceptance flow is broken. Check: is the invitation still pending? Is it expired? Is the token in the URL correct? Does the acceptance handler properly add the membership?"

**Organization switcher not updating data**
> "Claude, when I switch organizations, the data from the previous organization still shows. Check that the organization context updates and that all data queries re-fetch with the new organizationId."

---

## Reflection

You just built a multi-tenant SaaS application. Think about what that means: multiple users, multiple organizations, role-based permissions, and complete data isolation. This is the same architecture that powers Slack, Notion, and Linear.

The data model you designed this week -- users, organizations, memberships, projects, tasks -- is the backbone of most B2B SaaS products. The specific features change, but the multi-tenant, role-based structure stays the same.

Next week, you'll add the business model: Stripe payments with free and paid tiers.
