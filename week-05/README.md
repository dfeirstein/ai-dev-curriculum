# Week 5: Data, APIs, and Validation

**Theme:** Understand backends well enough to direct Claude Code on full-stack builds.

You've been building frontend-only sites -- pages that display content but don't store or retrieve data. This week that changes. You'll learn what databases, APIs, and validation are, why they matter, and how to direct Claude Code to build a full-stack application that stores real data.

By the end of the week you'll have a working bookmark manager: a real application where you can save, organize, search, and manage web bookmarks -- with data that persists in a database.

---

## Learning Objectives

1. Understand what a **database** is, how tables and relations work, and how to describe data models to Claude Code.
2. Understand what **Neon** provides as a hosted Postgres service and how to set up a connection.
3. Understand what an **ORM** (Drizzle) does and how to direct Claude Code on schemas and migrations.
4. Understand what **runtime validation** (Zod) is and why it matters for data integrity.
5. Understand what **API routes** are and how frontend and backend communicate.
6. Understand **environment variables** and why secrets never go in code.
7. Build and deploy a full-stack CRUD application.

## What You Ship

A **bookmark manager** -- a full-stack Next.js application with a Postgres database. Users can create, read, update, and delete bookmarks. Search and tag filtering. Deployed to Vercel with a live database.

## Time Commitment

~12-15 hours across the week.

## Prerequisites

- **Week 4 completed.** You have a rebuilt personal site using the full modern frontend stack.
- Comfortable directing Claude Code on Next.js, TypeScript, Tailwind, and shadcn/ui.
- A Vercel account for deployment.
- You'll need to create a free [Neon](https://neon.tech) account during this week's project.

## This Week's Materials

| File | What It Covers |
|------|---------------|
| [guide.md](guide.md) | Conceptual overview of databases, APIs, and validation -- read this first |
| [project.md](project.md) | Step-by-step project: build a bookmark manager |
| [resources.md](resources.md) | Links for deeper exploration |

## How to Approach This Week

1. **Read guide.md** carefully. This week introduces more new concepts than any previous week. Take your time.
2. **Work through project.md** step by step. The backend pieces require more setup than pure frontend work.
3. **Use resources.md** when you want deeper understanding of any concept.

This is the week where your projects stop being "websites" and start being "applications." The difference is data. Applications store, retrieve, and manipulate information. That's what you're learning to direct.
