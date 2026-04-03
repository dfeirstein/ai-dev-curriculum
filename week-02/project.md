# Week 2 Project: Multi-Page Personal Website

Build a 4-5 page personal website entirely through Claude Code direction. You will practice the Research --> Plan --> Execute workflow on every single page.

---

## What You Are Building

A professional personal website with these pages:

- **Home** -- upgraded from your Week 1 landing page
- **About** -- who you are, your background, what you are working on
- **Projects** -- showcase of 3-4 projects (placeholders are fine)
- **Blog** -- placeholder page for future posts
- **Contact** -- a contact form (does not need to actually send email yet)

All pages share a responsive navigation bar and consistent design.

---

## Step-by-Step Instructions

Everything below is done through Claude Code. You are directing. Claude is building.

### Step 1: Research

Open Claude Code in your Week 1 project directory and start with research:

> "Claude, what pages should a professional personal website have? What makes each page effective?"

> "Claude, what are best practices for navigation on a multi-page Next.js site?"

Read what Claude tells you. This is context gathering -- it will make your direction better in the steps that follow.

### Step 2: Plan

Ask Claude to plan the full site before building anything:

> "Claude, plan the site architecture for my personal website. I want these pages: Home (already exists), About, Projects, Blog (placeholder), Contact. Design the navigation structure, shared layout, and component hierarchy."

Review the plan carefully. Ask questions:

> "Why did you choose that layout approach?"
> "Should the navigation be in a shared layout or a separate component?"

Adjust the plan until you are satisfied. Then move to building.

### Step 3: Create CLAUDE.md

Before building any new pages, set up your project memory:

> "Claude, create a CLAUDE.md file for this project. Describe the tech stack, file structure, coding conventions, and quality standards based on what's already here."

Read what Claude creates. Edit anything that is wrong or missing. This file will guide Claude for the rest of the project.

### Step 4: Build the About Page

Follow the full branch workflow:

> "Claude, create a new branch called feature/about-page."

> "Claude, build an about page. Include a section about me (use placeholder text for now), my background, and what I'm currently working on. Match the style of the homepage."

Review in the browser. Iterate:

> "The text is too cramped. Add more spacing between sections."
> "Make the heading match the homepage heading style."

When you are happy with it:

> "Claude, commit these changes with a descriptive message, push the branch, and create a pull request."

> "Claude, merge the pull request and switch back to main."

### Step 5: Build the Projects Page

Same workflow, new branch:

> "Claude, create a branch called feature/projects-page."

> "Claude, build a projects page that displays 3-4 placeholder projects. Each project should have an image placeholder, a title, a short description, and a link. Use a card-based grid layout. Match the site's existing style."

Review. Iterate. Commit. PR. Merge. Back to main.

### Step 6: Build the Contact Page

Same workflow:

> "Claude, create a branch called feature/contact-page."

> "Claude, build a contact page with a form. Fields: name, email, and message. Include a submit button. The form doesn't need to actually send anything yet -- just the UI. Match the site's style."

Review. Iterate. Commit. PR. Merge. Back to main.

### Step 7: Build the Blog Placeholder

Same workflow:

> "Claude, create a branch called feature/blog-page."

> "Claude, build a blog page with a heading and a message that says 'Coming soon -- blog posts will appear here.' Match the site's style."

Review. Commit. PR. Merge. Back to main.

### Step 8: Add Navigation

This ties everything together:

> "Claude, create a branch called feature/navigation."

> "Claude, add a navigation bar that appears on all pages. It should have links to Home, About, Projects, Blog, and Contact. Make it responsive -- on mobile it should collapse into a hamburger menu."

Test on desktop and mobile (use browser dev tools to simulate mobile). Iterate until it feels right.

Commit. PR. Merge. Back to main.

### Step 9: Quality Gates

Run the full quality check:

> "Claude, run lint, type-check, and build. Fix any issues that come up."

Do a final visual review of every page. Check that navigation works between all pages. Check mobile responsiveness on every page.

### Step 10: Deploy

Push main to GitHub. If Vercel is connected, your site deploys automatically. Visit the live URL and verify everything works in production.

---

## Acceptance Criteria

- [ ] 4 or more pages, all accessible via navigation
- [ ] CLAUDE.md exists in the project root and accurately describes the project
- [ ] Clean pull request history -- one PR per feature, all merged to main
- [ ] Responsive design on mobile for all pages
- [ ] `npm run build` passes with no errors
- [ ] Live and working on Vercel

---

## Stretch Goals

If you finish early and want to push further:

- **Page transitions:** "Claude, add smooth transitions when navigating between pages."
- **Dark mode toggle:** "Claude, add a dark mode toggle to the navigation bar. Remember the user's preference."
- **Blog with real posts:** "Claude, set up the blog page to render markdown files from a /posts directory."
- **RSS feed:** "Claude, add an RSS feed for the blog."
- **SEO metadata:** "Claude, add proper meta tags, Open Graph images, and a sitemap."
