# Week 19 Guide: Building a Professional Portfolio

---

## Part 1: What Makes a Great Developer Portfolio

### Live Projects Beat Descriptions

The single most important thing in a developer portfolio is live, working projects. Not descriptions of projects. Not screenshots alone. Live URLs that someone can click and use.

When a hiring manager or potential client looks at your portfolio, they want to answer one question: "Can this person build real things?" A live demo answers that instantly. A description does not.

You have multiple live projects. That's your biggest advantage. Most developer portfolios are full of projects that are "private" or "in progress" or "available on request." Yours are live. Use that.

### Show Range

Your portfolio demonstrates something unusual: range. You've built:

- **A SaaS application** (TeamTask Pro) -- multi-tenant, authenticated, with payments, notifications, dashboards, testing, monitoring, and CI/CD.
- **An independent project** -- your own idea, your own decisions, production-ready.
- **A speed build** -- proof you can ship fast.
- **An open source contribution or npm package** -- proof you can work with other people's code or create reusable tools.

That's not just "I can build websites." That's "I can build anything, and I can do it at professional speed." Make sure your portfolio tells that story.

### The Visitor's Journey

Think about how someone experiences your portfolio:

1. They land on your portfolio site. Within 5 seconds, they should understand who you are and what you do.
2. They scan your projects. Each project should have a compelling title, a one-line description, and a screenshot.
3. They click into a project. They should see a live demo link, a clear description of what it does, and the tech stack used.
4. They explore the demo. It should work. Nothing undermines credibility like a broken demo.
5. They read more. A blog post about your process shows depth of thinking.

Every step should be frictionless. If they have to guess what a project does, you've lost them.

---

## Part 2: README Writing

### The README Is Your Project's Landing Page

When someone visits your GitHub repository, the README is the first thing they see. A great README makes your project look professional, intentional, and well-maintained. A missing or thin README makes even excellent code look like an abandoned experiment.

### Anatomy of a Great README

**Title and description (2 lines):**
What is this? One sentence. Be specific.

Bad: "A web application built with Next.js."
Good: "A multi-tenant task management SaaS with team workspaces, Stripe billing, and real-time notifications."

**Screenshot or demo GIF:**
Show the app. One screenshot of the main view. If you have a GIF of the core flow in action, even better.

**Live demo link:**
Big, obvious, unmissable. The first thing someone should be able to click.

**Features (bullet list):**
What does the app do? 5-8 bullet points, each one sentence. Focus on what the user can do, not what technology is used.

Bad: "Uses Drizzle ORM for database queries."
Good: "Organize tasks across multiple team workspaces with drag-and-drop."

**Tech stack:**
A clean list of technologies. Group them logically: frontend, backend, database, auth, payments, testing, monitoring, deployment.

**Getting started:**
Step-by-step instructions for running the project locally. Clone, install, configure environment variables, run. Someone should be able to follow these instructions exactly and have the project running in 5 minutes.

**Environment variables:**
List every required environment variable with a description of what it's for. Never include actual secret values.

### Writing READMEs with Claude Code

> "Rewrite the README.md for this project. Make it professional and compelling. Include: a specific one-sentence description, a screenshot placeholder, the live demo link at [URL], a features list focused on user capabilities, the tech stack, detailed getting-started instructions, and environment variable documentation."

Review what Claude Code writes. Is the description accurate? Are the features listed correctly? Does the getting-started section actually work? Don't trust it blindly -- verify.

---

## Part 3: Screenshots and Visuals

### Why Visuals Matter

People process images faster than text. A screenshot of your app tells someone more in 2 seconds than a paragraph of description tells them in 20 seconds.

### Taking Good Screenshots

- **Use a clean browser window.** Hide bookmarks bar, extensions, and other clutter.
- **Show the app with realistic data.** Not empty states. Not lorem ipsum. Real-looking data that shows what the app actually does.
- **Capture the most impressive view.** The dashboard with data, the main feature in action, the best-looking page.
- **Use consistent dimensions.** All screenshots for one project should be the same size.
- **Consider light mode.** Dark mode looks great to developers but can be harder to read in a README.

### Demo Videos

For your top 2 projects, record a 2-minute walkthrough video. Here's the formula:

1. **0:00-0:15 -- Introduction.** "This is [project name]. It lets you [one-sentence description]."
2. **0:15-1:15 -- Core feature demo.** Show the main thing the app does. Click through it naturally.
3. **1:15-1:45 -- Supporting features.** Quickly show 2-3 other capabilities.
4. **1:45-2:00 -- Wrap up.** "Built with [tech stack]. Check it out at [URL]."

You can record these with QuickTime on macOS (File > New Screen Recording) or any screen recording tool. You don't need editing software -- just record it in one take. A slightly imperfect 2-minute video is infinitely better than no video at all.

---

## Part 4: Writing About Your Work

### The Power of Writing

Writing about your projects does three things:

1. **It demonstrates depth.** Anyone can build a thing. Writing about the decisions you made, the problems you solved, and the trade-offs you considered shows you can think, not just execute.
2. **It's discoverable.** Blog posts show up in search results. Someone googling "how to build a SaaS with Next.js" might find your post.
3. **It compounds.** Every post you write is another node in your professional network. Over time, these add up.

### What to Write About

Pick one:

- **"How I Built [Project Name] in [Timeframe] with AI"** -- Walk through your process. What did you build? What decisions did you make? What surprised you? What would you do differently?
- **"What I Learned Building a SaaS from Scratch"** -- Broader reflection on the TeamTask Pro experience. Auth, payments, multi-tenancy -- what concepts matter most?
- **"My Journey from Zero to AI-Native Builder"** -- The full story. Where you started, what you learned, where you are now.

Structure: introduction (what this is about), the story (chronological or thematic), key takeaways (what the reader can learn), conclusion (what's next for you).

Write it naturally. You're not writing a textbook. You're telling someone about your experience. Conversational tone is good.

---

## Part 5: The Portfolio Site

### Updating Your Portfolio

You built a portfolio site in Week 7. Now it needs to showcase everything you've built since then. Direct Claude Code to update it:

> "Update my portfolio site to showcase these projects: [list each project with its name, description, live URL, and GitHub URL]. Each project should have a screenshot, a description, the tech stack, and links to both the live demo and the source code."

### The About Page

Your about page should tell people:
- Who you are (in a few sentences)
- What you do (AI-native builder -- you research, plan, and direct AI to build production software)
- What you're looking for (if anything -- job, freelance clients, collaborators, just building in public)
- How to contact you

### The Blog

Add your blog post to the portfolio site's blog section. If you write more over time, this becomes a genuine content archive that builds your professional presence.

---

## Part 6: The Meta-Skill

### Marketing Yourself as an AI-Native Builder

You have a skill that is genuinely rare in 2026: you can take a vague idea and turn it into a production application, fast, using AI as your primary tool. That's a new category of professional, and the market is still figuring out how to value it.

Here's how to present yourself:

**Don't say:** "I use ChatGPT to code."
**Do say:** "I build production software by directing AI tools through a research, plan, execute, and verify workflow."

**Don't say:** "I can't write code."
**Do say:** "I focus on architecture, product decisions, and quality verification. AI handles the implementation."

**Don't say:** "I'm learning to be a developer."
**Do say:** "I'm an AI-native builder with [X] shipped projects in production."

The framing matters. You're not an AI user. You're a builder who leverages AI as a force multiplier. The projects prove it.
