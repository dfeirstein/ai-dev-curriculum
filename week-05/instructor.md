# Week 5: Data, APIs, and Validation — Instructor Guide

## Week at a Glance
- Theme and key concepts: First full-stack application. Students learn how data flows from a form on the frontend through an API route to a Postgres database and back. Drizzle ORM for database access, Zod for input validation, Neon for hosted Postgres.
- What students ship: A bookmark manager with full CRUD (create, read, update, delete), tagging, search, and a real Postgres database — deployed live
- Estimated hours: 8-10
- Difficulty: 4

## Pacing Guide
- **Day 1-2:** Read guide.md. Set up Neon Postgres account and database. Research the data model with Claude. Define the schema with Drizzle. Run the first migration. Verify the database has tables.
- **Day 3-4:** Build CRUD API routes and connect the frontend. Create bookmark, list bookmarks, edit, delete. Add Zod validation on all inputs. Add tagging and search/filter.
- **Day 5:** Error handling, loading states, empty states. Polish the UI. Test edge cases (empty fields, duplicate URLs, long titles). Deploy with the database connection string set in Vercel environment variables.

## Getting Students Started
- Kick off by drawing the data flow on a whiteboard or shared screen: User clicks "Save Bookmark" -> Frontend sends data to API route -> API route validates with Zod -> Drizzle writes to Postgres -> Response flows back. This is the mental model for the entire week. Every bug they encounter maps to one of these steps.
- Common confusion: students do not understand why the database is separate from the app. Explain that Neon is a database service running somewhere else on the internet. The app connects to it with a URL, like connecting to any website.
- Students should have open: Ghostty terminal, browser with the dev server, Neon dashboard (to see their database tables), and the project.md for step-by-step instructions.

## Check-in Points
- **After guide.md:** Can the student trace the full data flow? Ask: "When a user saves a bookmark, what happens step by step?" They should mention: form submission, API route, validation, database write, response. If they cannot trace this, spend more time on the conceptual model before building.
- **Mid-project checkpoint:** The database has tables (check Neon dashboard). At least Create and Read work — the student can save a bookmark and see it appear in a list. If the database is empty or unconnected, they are significantly behind. Common blocker: the `DATABASE_URL` environment variable is not set or is wrong.
- **Pre-ship review:** All four CRUD operations work. Zod validation catches bad input (try submitting an empty form, a non-URL string). Search or filter works. The app is deployed to Vercel with the database connected (not just running locally).

## Common Pitfalls
- **Database connection failures.** This is the number one blocker in Week 5. The `DATABASE_URL` is wrong, missing, or not available in the right environment. Checklist: Is it in `.env.local`? Is it in Vercel environment variables? Is the Neon database actually running (not paused)? Does the connection string include `?sslmode=require`?
- **Not understanding frontend-to-backend data flow.** Student builds the UI and the database schema but cannot connect them. They don't understand that an API route is the bridge. Draw the flow diagram again. Have Claude explain what `fetch('/api/bookmarks', { method: 'POST', body: ... })` does.
- **Skipping Zod validation.** Student gets CRUD working and considers it done. Push them: "What happens if someone submits an empty URL? What if the title is 10,000 characters?" Validation is not optional — it is part of the build.
- **Migration confusion.** Student changes the schema but forgets to run `drizzle-kit push` or `drizzle-kit generate`/`migrate`. Their code expects columns that don't exist in the database. If they see "column does not exist" errors, the fix is almost always running the migration.
- **Deploying without setting environment variables on Vercel.** The app works locally but crashes in production. This is always the `DATABASE_URL` not being set in Vercel's environment variable settings. Teach them to check Vercel dashboard -> Settings -> Environment Variables.

## How to Know the Student Gets It
- They can trace a bug to the right layer. "The form submits but nothing appears in the list" — they know to check the API route and database, not the frontend rendering. "The form shows an error on valid input" — they know to check the Zod schema.
- They can explain what Drizzle does vs what Postgres does vs what Zod does. Drizzle talks to the database in TypeScript instead of raw SQL. Postgres stores the data. Zod validates data before it reaches the database.
- When they add a new feature (like tagging), they instinctively think about all layers: schema change, migration, API route update, Zod schema update, frontend update.
- Red flags: Student has a working UI with hardcoded data (no real database connection). Student cannot explain what an API route is. Student's Neon dashboard shows no tables.

## Support Strategies
- If stuck on database connection: Walk through the `DATABASE_URL` step by step. Copy it from Neon dashboard. Paste it into `.env.local`. Restart the dev server. Run the migration. Test with a simple query. This is almost always the fix.
- If rushing without reviewing: Ask them to open the Neon dashboard and show you the data in the tables. Ask them to submit a bookmark with invalid data and show you what happens. These two checks verify the full stack is actually working.
- If frustrated: This is the hardest week so far, and the jump from static sites to full-stack is real. Normalize the difficulty. Focus on getting Create and Read working first — that is the breakthrough moment. Update and Delete are mechanically similar once Create/Read work.

## Differentiation
- **Moving fast?** Add bookmark import/export (JSON or CSV). Add favicon fetching for bookmarked URLs. Add sorting by date, title, or tag. Add pagination for large bookmark lists.
- **Struggling?** Reduce to Create and Read only — skip Update and Delete. Skip search and tagging. A working form that saves bookmarks to a database and displays them is a major accomplishment.
- **Minimum viable ship:** A deployed app where you can add a bookmark and see it in a list, backed by a real Postgres database. Data persists across page refreshes. That is the full-stack milestone.
