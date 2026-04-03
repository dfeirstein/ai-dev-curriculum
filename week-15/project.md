# Project: Choose Your Own -- MVP Build

**What you're building:** An MVP of a project you choose. Something you'd actually use.

**Time estimate:** 15-20 hours across the week

**What you'll need open:** Ghostty, your browser, and your research notes

---

## The Brief

This is your project. You pick the idea, you scope the features, you make the architecture decisions, and you direct Claude Code to build it. By the end of the week, you'll have a deployed MVP with core functionality working.

---

## Project Ideas

If you don't have your own idea yet, pick one of these. Each is scoped to be achievable in two weeks and uses the stack you already know.

1. **Booking/Scheduling App** -- Let users create available time slots and let others book them. Think Calendly, but simpler.
2. **Content Platform** -- A place to write and publish articles with rich text editing, categories, and a public reading view.
3. **Marketplace** -- A two-sided platform where users can list items and others can browse and contact sellers.
4. **Budget Tracker** -- Track income and expenses, categorize spending, see monthly summaries and trends.
5. **Habit Tracker** -- Define daily habits, check them off, see streaks and completion rates over time.
6. **Job Board** -- Companies post jobs, candidates browse and apply. Filter by category, location, remote status.
7. **Recipe Manager** -- Save recipes from anywhere, organize by category, search by ingredient or name.
8. **Link-in-Bio Tool** -- A customizable page of links, like Linktree, with analytics showing click counts.
9. **Event RSVP App** -- Create events, share invite links, collect RSVPs, send reminders.
10. **Study Flashcard App** -- Create decks of flashcards, study with spaced repetition, track progress.

Pick one that excites you. If none of these spark joy, use your own idea -- just make sure it's scoped to an MVP you can build in a week.

---

## Day 1-2: Research Phase

### Step 1: Competitive Analysis

Before you build anything, research what exists.

> "Claude, I want to build a [your project idea]. Search for and list the top 5 existing tools that do something similar. For each one, describe what they do well and what common complaints users have."

Now go use 2-3 of those tools yourself. Sign up, click around, try the core features. Take notes:

- What felt good about the experience?
- What was frustrating or confusing?
- What's missing that you wish was there?

### Step 2: Define Your Feature Set

Write down three lists:

**Must-Have (this week):**
- [ ] Core feature 1 (the main thing your app does)
- [ ] Core feature 2 (if needed for the core to work)
- [ ] User authentication (if users need accounts)
- [ ] Basic UI that's functional and clear

**Nice-to-Have (next week):**
- [ ] Polish and visual refinement
- [ ] Secondary features
- [ ] Settings or preferences
- [ ] Email notifications

**Later (post-curriculum):**
- [ ] Advanced features
- [ ] Integrations
- [ ] Mobile optimization
- [ ] Social features

### Step 3: Map User Flows

For each must-have feature, write out the user flow:

1. User arrives at the site. What do they see?
2. User signs up / logs in. Where do they land?
3. User performs the core action. What steps do they take?
4. User sees the result. What does it look like?

These flows become your building roadmap.

---

## Day 3: Plan Phase

### Step 4: Architecture Planning with Claude Code

Start Claude Code and plan your project:

> "I'm building a [project name] -- a [one-sentence description]. Here are the must-have features: [list them]. I want to use Next.js 15 with the App Router, TypeScript, Tailwind CSS, shadcn/ui, Postgres with Drizzle ORM, and Better Auth. Help me plan: what database tables do I need, what pages should the app have, and how should the project be organized?"

Review Claude Code's plan carefully. Ask follow-up questions:

> "Is this the simplest possible schema for the MVP? Can we cut anything?"

> "Walk me through the user flow for [core feature]. Does this page structure support it?"

### Step 5: Set Up the Project

Once you're happy with the plan, start building:

> "Create a new Next.js project called [your-project-name] with TypeScript, Tailwind CSS, and the App Router. Set up the project structure based on our plan."

Then, one by one:

> "Set up Postgres with Drizzle ORM. Create the database schema we planned: [list the tables]."

> "Set up Better Auth with email/password authentication. Add sign-in and sign-up pages."

> "Set up the basic layout with a navigation bar, sidebar if needed, and responsive design using shadcn/ui components."

Verify each step works before moving on. Run the dev server, check the browser, confirm auth works.

---

## Days 4-5: Build Sprint

### Step 6: Build Core Features

Now build your must-have features one at a time. For each feature:

1. **Describe it clearly to Claude Code:**
   > "Build the [feature name]. Here's how it should work: [describe the user flow]. Use [specific UI components] for the interface. Store the data in the [table name] table we created."

2. **Verify it works:**
   - Open the browser and test the feature
   - Try the happy path (everything works as expected)
   - Try edge cases (empty inputs, missing data, unexpected actions)
   - Does it look reasonable? Not perfect -- reasonable.

3. **Commit the working feature:**
   > "Commit this with the message: Add [feature name] with [brief description]"

4. **Move to the next feature.**

### Step 7: Deploy Early

Don't wait until everything is done. Deploy as soon as you have auth and at least one feature working:

> "Set up this project for deployment on Vercel. Create the GitHub repository, push the code, and connect it to Vercel. Make sure the environment variables are configured for the database and auth."

Having a live URL early catches deployment issues before they become last-minute emergencies.

### Step 8: Keep Building

Continue building must-have features. Stay disciplined:

- **Only build must-have features this week.** Nice-to-haves are next week.
- **If a feature is taking too long, simplify it.** A working simple version beats a broken complex version.
- **Commit after every working feature.** Small commits, frequent saves.
- **Check the deployed version periodically.** Make sure it works on Vercel, not just localhost.

---

## Acceptance Criteria

By the end of Week 15, your project must meet these requirements:

- [ ] **Deployed to Vercel** with a live, working URL
- [ ] **Built with Next.js, TypeScript, and Postgres** (your standard stack)
- [ ] **Authentication working** (if your project requires user accounts)
- [ ] **Core functionality implemented** -- the main thing your app does works
- [ ] **Data persists** -- if a user creates something, it's there when they come back
- [ ] **No crashes on basic usage** -- the happy path works without errors
- [ ] **Code is on GitHub** with meaningful commit history

This is an MVP. It doesn't need to be beautiful. It doesn't need to handle every edge case. It needs to work. Beauty and robustness come next week.

---

## Gut Check

Ask yourself:

- If I showed this to a friend, could they understand what it does?
- Could they use the core feature without my help?
- Does it work, even if it's rough around the edges?

If the answers are yes, you've built a successful MVP. That's a real achievement. Many professional developers struggle to ship an MVP in a week. You just did it by directing an AI.

Next week, you'll make it production-ready.
