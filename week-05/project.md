# Project: Bookmark Manager

**What you're building:** A full-stack web application for saving, organizing, searching, and managing web bookmarks. Backed by a real database. Deployed live.

**Time estimate:** 8-10 hours

**What you'll need open:** Ghostty (your terminal), a web browser, and a Neon account

---

## The Brief

You're building your first real application -- not a website, an application. It stores data. It has CRUD operations (Create, Read, Update, Delete). It validates user input. It talks to a database.

When you're done, you'll have a live URL where you can save bookmarks, tag them, search through them, and organize them. The data persists -- close the browser, come back later, your bookmarks are still there.

---

## Phase 1: Research

Start Claude Code and research the architecture:

> "Claude, I'm building a bookmark manager as a full-stack Next.js app. It needs to save, organize, search, and manage web bookmarks. The tech stack is Next.js App Router, TypeScript strict mode, Tailwind, shadcn/ui, Drizzle ORM, Postgres on Neon, and Zod validation. What features should it have? What should the data model look like?"

Follow up:

> "What are the best practices for structuring API routes in Next.js for a CRUD app?"

> "How should I handle search and filtering with Drizzle?"

> "What shadcn/ui components would work well for a bookmark manager -- list views, forms, search, tags?"

---

## Phase 2: Plan

Ask Claude Code to create a detailed plan:

> "Claude, plan a bookmark manager with these requirements: Postgres database on Neon, Drizzle ORM for data access, Zod validation on all inputs, Next.js API routes for CRUD, and a frontend with shadcn/ui. The bookmark model should have: title, URL, description (optional), tags (multiple), and timestamps. Include search by title/description and filtering by tag. Plan the data model, API routes, and frontend pages before building anything."

Review the plan carefully. This is a more complex project than previous weeks. Make sure you understand:

- What tables will be created?
- What API endpoints will exist?
- What pages will the frontend have?
- How will search and filtering work?

Approve the plan before moving on.

---

## Phase 3: Execute

### Step 1: Create the Project

> "Claude, create a new Next.js project called bookmark-manager with TypeScript strict mode, Tailwind CSS, App Router, shadcn/ui, ESLint, and Prettier. Set up the full configuration."

### Step 2: Set Up the Database

First, create your Neon database:

1. Go to [neon.tech](https://neon.tech) and create a free account
2. Create a new project (any name is fine)
3. Copy the connection string from the dashboard

Now tell Claude Code:

> "Claude, set up Drizzle ORM with Postgres. Create a .env.local file with DATABASE_URL set to my connection string. Make sure .env.local is in .gitignore. Install the Drizzle dependencies."

Paste your connection string when Claude Code asks for it (or add it to .env.local yourself).

### Step 3: Define the Data Model

> "Claude, create a Drizzle schema for bookmarks with: id (auto-generated), title (required, max 200 chars), url (required, valid URL), description (optional, max 1000 chars), tags (array of strings), createdAt (auto-generated timestamp), and updatedAt (auto-generated timestamp). Then run the migration to create the table in Neon."

Verify the migration ran successfully. Ask Claude Code:

> "Claude, verify the database connection works and the bookmarks table exists."

### Step 4: Create the API Routes

> "Claude, create API routes for bookmarks: GET /api/bookmarks (list all, with search and tag filter query params), GET /api/bookmarks/[id] (get one), POST /api/bookmarks (create -- validate with Zod), PUT /api/bookmarks/[id] (update -- validate with Zod), DELETE /api/bookmarks/[id] (delete). Return proper status codes. Handle errors gracefully."

### Step 5: Build the Frontend -- List View

> "Claude, build the main bookmarks page at /bookmarks. Show all bookmarks in a list using shadcn/ui Cards. Each card shows the title (linked to the URL), description, tags as Badges, and the date it was saved. Add a search input at the top that filters bookmarks by title or description. Add a tag filter sidebar or dropdown."

Check the browser. Can you see the bookmarks page? The list will be empty -- that's correct, you haven't added any bookmarks yet.

### Step 6: Build the Frontend -- Add Bookmark Form

> "Claude, add an 'Add Bookmark' button that opens a shadcn/ui Dialog with a form. Fields: title, URL, description (optional), tags (comma-separated or multi-select). On submit, call the POST API route and refresh the list. Show validation errors from Zod if the input is invalid."

Test it. Add a bookmark. Does it show up in the list? Try submitting an invalid URL. Does the validation catch it?

### Step 7: Build Edit and Delete

> "Claude, add edit and delete functionality to each bookmark card. Edit should open the same Dialog form pre-filled with the bookmark's data. Delete should show a confirmation dialog before removing. Both should update the list after the operation."

Test all operations:
- Create a bookmark
- Edit its title
- Change its tags
- Delete it
- Verify it's gone

### Step 8: Polish the UI

> "Claude, polish the bookmark manager UI. Add: empty state when there are no bookmarks yet (a friendly message with an 'Add your first bookmark' button), loading states while data fetches, proper spacing and typography, and make sure everything is responsive on mobile."

### Step 9: Quality Gates

Run the full pipeline:

> "Claude, run npm run lint and fix any issues."

> "Claude, run npm run typecheck and fix any issues."

> "Claude, run npm run build and make sure it completes without errors."

All three must pass.

### Step 10: Deploy

> "Claude, initialize a git repo, commit everything, create a GitHub repo called bookmark-manager, and push."

Before deploying, you need to set up the database connection in production:

> "Claude, deploy to Vercel. I need to set the DATABASE_URL environment variable in the Vercel dashboard. Walk me through it."

Claude Code will guide you through adding the environment variable to Vercel. Once that's done, deploy.

Visit your live URL. Test everything:
- Add a bookmark
- Search for it
- Edit it
- Delete it
- Check on mobile

---

## Acceptance Criteria

Your project is complete when all of these are true:

- [ ] App is live on Vercel at a public URL
- [ ] Built with Next.js, TypeScript strict mode, Tailwind, shadcn/ui, Drizzle, Postgres (Neon), Zod
- [ ] Can create bookmarks with title, URL, description, and tags
- [ ] Can view all bookmarks in a list
- [ ] Can search bookmarks by title or description
- [ ] Can filter bookmarks by tag
- [ ] Can edit existing bookmarks
- [ ] Can delete bookmarks (with confirmation)
- [ ] Invalid inputs are rejected with clear error messages
- [ ] Has empty state, loading states, and error handling
- [ ] Responsive design -- works on mobile and desktop
- [ ] `npm run lint` passes
- [ ] `npm run typecheck` passes
- [ ] `npm run build` completes without errors
- [ ] Database connection string is in environment variables, not in code
- [ ] Code is on GitHub in a public repository

---

## Stretch Goals

**Import Bookmarks from Browser**
> "Claude, add a feature to import bookmarks from a browser export file (HTML format). Parse the file and create bookmarks in the database."

**Add Link Previews**
> "Claude, when a user saves a bookmark, fetch the page title, description, and favicon from the URL automatically. Show the favicon next to each bookmark in the list."

**Add Categories**
> "Claude, add a categories feature. Users can create categories and assign bookmarks to them. Add a category filter to the sidebar."

**Add Sorting**
> "Claude, add sort options to the bookmark list: sort by date created, date modified, or title. Add a dropdown to select the sort order."

---

## Troubleshooting

**Database connection fails**
> "Claude, the database connection is failing with this error: [paste error]. Check the DATABASE_URL in .env.local and the Neon dashboard."

**Migration errors**
> "Claude, the Drizzle migration failed with this error: [paste error]. Fix it and re-run the migration."

**API routes return 500 errors**
> "Claude, the POST /api/bookmarks route is returning a 500 error. Check the route handler for errors and add better error logging."

**Data doesn't persist after deploy**
> "Claude, bookmarks work locally but don't show up on the deployed version. Check that the DATABASE_URL environment variable is set correctly in Vercel."

---

## Reflection

You just built a full-stack application. Data flows from the browser to the server to a database and back. You didn't write the SQL, the API handlers, the validation logic, or the data fetching code. You described the data model, the operations, and the user experience -- and directed Claude Code to build all of it.

This is the same architecture behind every web application you use daily. The bookmark manager is simple, but the pattern is identical to how Twitter stores tweets, how Notion stores documents, and how Stripe stores transactions. You now understand that pattern well enough to direct its construction.
