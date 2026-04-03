# Week 5 Guide: Data, APIs, and Validation

Until now, everything you've built has been static. Your personal site displays content, but it doesn't remember anything. There's no data being saved, retrieved, or changed. This week, your projects become dynamic. You'll understand how data flows through a real application -- from the user's browser, through your server, into a database, and back again.

You won't write any of this code. But you need to understand the concepts deeply enough to give Claude Code precise architectural direction.

---

## Part 1: PostgreSQL -- Where Your Data Lives

### What a Database Is

A database is a structured place to store and retrieve information. Think of it like a spreadsheet, but built for software -- it can handle millions of rows, multiple users accessing it simultaneously, and complex queries like "show me all bookmarks tagged 'design' created in the last week, sorted by date."

### The Building Blocks

**Tables** are like individual sheets in a spreadsheet. Each type of data gets its own table. A bookmark manager might have tables for bookmarks, tags, and categories.

**Rows** are individual entries. One bookmark. One tag. One category. Each row is a record.

**Columns** are the fields for each entry. A bookmark row might have columns for title, URL, description, tags, and created_at (when it was saved).

**Relations** are connections between tables. A bookmark can have many tags. A category can contain many bookmarks. These connections are how data stays organized and consistent.

### Why PostgreSQL

Postgres (short for PostgreSQL) is the industry standard for relational databases. It's reliable, powerful, free, and what the vast majority of production applications use. When you tell Claude Code to set up a database, Postgres is the default choice.

### Your Role

You don't write SQL (the language databases use). You describe your data to Claude Code:

> "Create a Postgres table for bookmarks with title, URL, description, tags, and timestamps."

> "I need a categories table. Each bookmark belongs to one category."

> "Add a 'priority' column to the bookmarks table -- high, medium, or low."

Claude Code translates your data descriptions into the right database structure.

---

## Part 2: Neon -- Hosted Postgres

### Why Hosted

Running a database on your laptop works for development, but it has problems: the data disappears when you close your laptop, other people can't access it, and you can't deploy an app that depends on it.

Neon gives you a Postgres database in the cloud. It's always running, accessible from anywhere, and has a free tier that's more than enough for learning projects.

### What You Get

When you create a Neon database, you get a **connection string** -- a URL that lets your application connect to the database. It looks something like:

```
postgresql://username:password@host/database
```

You give this connection string to your application (through an environment variable -- more on that later). Claude Code handles the rest.

### Setting It Up

1. Create a free account at neon.tech
2. Create a new project
3. Copy the connection string
4. Store it as an environment variable

That's it. Four steps. Claude Code will guide you through the details.

---

## Part 3: Drizzle ORM -- How Code Talks to the Database

### What an ORM Does

ORM stands for "Object-Relational Mapping." It's a translator between your application and your database. Without an ORM, your application would need to speak raw SQL to the database. With an ORM, your application works with familiar data structures, and the ORM translates those into database operations behind the scenes.

### Why Drizzle

Drizzle is the ORM you'll use because it has three qualities that matter for AI-native development:

1. **Type-safe:** It integrates with TypeScript, so `npm run typecheck` catches database-related bugs too.
2. **SQL-like:** It doesn't hide what's happening. The operations map closely to actual database operations, which means fewer surprises.
3. **Lightweight:** It doesn't add unnecessary complexity.

### Schemas

A schema defines the structure of your database tables in code. When Claude Code creates a schema, it's declaring what tables exist, what columns they have, and how they relate to each other.

When directing Claude Code: "Use Drizzle ORM with Postgres. Define schemas for bookmarks with title, URL, description, tags, and timestamps."

### Migrations

When you change your schema -- adding a column, renaming a field, creating a new table -- the database needs to be updated to match. A migration is the process of applying those changes.

Think of it like this: the schema is the blueprint, and the migration is the renovation. You change the blueprint, then run the migration to make the actual building match.

When directing Claude Code: "Add a 'priority' column to the bookmarks table and run the migration."

Claude Code handles the migration mechanics. You just describe what changed.

---

## Part 4: Zod -- Runtime Validation

### The Problem

TypeScript catches bugs at build time -- before your app runs. But there's a gap: user input arrives at runtime. A user could submit anything through a form. An API could receive any data. TypeScript can't help here because the data doesn't exist until the app is running.

### What Zod Does

Zod validates data at runtime. You define rules for what valid data looks like, and Zod enforces them when data arrives.

For example, a bookmark submission must have:
- A title (text, between 1 and 200 characters)
- A URL (valid URL format)
- A description (text, optional, max 1000 characters)

If a submission doesn't match these rules, Zod rejects it before it ever touches your database. This protects your data integrity.

### Why It Matters

Without validation, garbage goes in and garbage comes out. A user could submit a bookmark with no URL. An empty title. A description that's 50,000 characters long. Zod prevents all of this.

When directing Claude Code: "Validate all user inputs with Zod schemas. Bookmark creation requires a title (1-200 chars) and a valid URL. Description is optional."

### The TypeScript + Zod Pipeline

Together, TypeScript and Zod give you two quality gates:

- **TypeScript** ensures your code is structurally sound at build time.
- **Zod** ensures incoming data is valid at runtime.

Between the two, entire categories of bugs become impossible.

---

## Part 5: API Routes -- How Frontend Talks to Backend

### The Concept

Your web application has two sides:

- **Frontend:** What the user sees and interacts with in their browser. The pages, buttons, forms.
- **Backend:** The server-side logic that processes data, talks to the database, and returns results.

The frontend and backend communicate through API routes -- specific URLs that the frontend can send requests to and receive responses from.

### REST: The Convention

REST is a convention for organizing these requests. You don't need to know the technical details -- just the verbs:

- **GET:** Read data. "Give me all bookmarks." "Give me bookmark #42."
- **POST:** Create data. "Save this new bookmark."
- **PUT:** Update data. "Change the title of bookmark #42."
- **DELETE:** Remove data. "Delete bookmark #42."

### Status Codes

When the backend responds, it includes a status code that tells the frontend what happened:

- **200:** Success. Here's what you asked for.
- **201:** Created. The new thing was saved successfully.
- **400:** Bad request. Something was wrong with what you sent.
- **404:** Not found. That thing doesn't exist.
- **500:** Server error. Something broke on our end.

### In Next.js

API routes in Next.js live in `app/api/` folders. The folder structure maps to the URL, just like pages:

- `app/api/bookmarks/route.ts` handles requests to `/api/bookmarks`
- `app/api/bookmarks/[id]/route.ts` handles requests to `/api/bookmarks/42`

When directing Claude Code: "Create API routes for CRUD operations on bookmarks. Use GET for listing and reading, POST for creating, PUT for updating, DELETE for removing. Validate all inputs with Zod."

---

## Part 6: Environment Variables -- Keeping Secrets Safe

### The Problem

Your database connection string contains a username and password. Your API keys are sensitive credentials. If these end up in your code and you push to GitHub, anyone can see them. This is a serious security issue.

### The Solution

Environment variables store sensitive information outside your code. They live in a file called `.env.local` on your machine and in the Vercel dashboard for production.

The `.gitignore` file (which Claude Code sets up) ensures `.env.local` never gets pushed to GitHub. Your secrets stay local.

### How It Works

1. You create `.env.local` with your connection string: `DATABASE_URL=postgresql://...`
2. Your application reads from `DATABASE_URL` instead of having the connection string hardcoded.
3. In production (Vercel), you set the same variable in the dashboard.

When directing Claude Code: "Set up environment variables for the database connection string. Use .env.local for development. Make sure .env.local is in .gitignore."

### The Rule

**Secrets never go in code. Ever.** If Claude Code puts a connection string or API key directly in a file (not an environment variable), that's a mistake. Catch it. Tell Claude Code to use an environment variable instead.

---

## Part 7: The Full Picture

Here's how all the pieces connect in a single operation -- say, saving a new bookmark:

1. **User** fills out a form in the browser (frontend, built with shadcn/ui)
2. **Form** sends the data to an API route (POST to `/api/bookmarks`)
3. **Zod** validates the incoming data -- is the title present? Is the URL valid?
4. **Drizzle** takes the validated data and saves it to the database
5. **Postgres** (on Neon) stores the bookmark permanently
6. **API route** sends back a success response (201 Created)
7. **Frontend** updates to show the new bookmark in the list

Data flows from user to browser to server to database and back. Every piece has a job. When you understand this flow, you can direct Claude Code to build any data-driven application.

---

## What's Next

You understand the full stack now -- frontend and backend. Head to [project.md](project.md) to build your first full-stack application: a bookmark manager.
