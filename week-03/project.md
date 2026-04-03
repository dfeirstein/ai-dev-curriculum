# Week 3 Project: Supercharge Your Development Environment

Take your Week 2 personal website and add a professional-grade development setup. By the end, you'll have custom skills, automated quality hooks, and a comprehensive CLAUDE.md -- and you'll prove it all works by adding a new page using your custom workflow.

**Remember: you do not write any code, configuration, or files by hand. You direct Claude Code to do everything.**

---

## Step 1: Enhance Your CLAUDE.md

Your Week 2 CLAUDE.md was a starting point. Now you make it comprehensive.

Open Claude Code in your project directory and direct it:

> "Claude, update the CLAUDE.md with detailed coding conventions, project structure documentation, and quality standards for this project. Include the stack we're using, the file organization, naming conventions, styling approach, and the quality checks that should pass before any code ships."

Review what Claude produces. A good CLAUDE.md should cover:
- The tech stack and why each piece is there
- Project structure (what lives where)
- Naming conventions (files, components, variables)
- Styling approach (how Tailwind is used, design tokens, responsive patterns)
- Quality standards (what must pass: lint, type-check, build)
- Git workflow (branch naming, commit messages, PR process)

If anything is missing or vague, tell Claude to improve it. The CLAUDE.md should be specific enough that a brand-new Claude session could pick up your project and know exactly how things work.

---

## Step 2: Install Useful Skills

Explore what's available:

> "Claude, what skills are available that would help with a Next.js project? Show me the most useful ones for development workflow, quality assurance, and deployment."

Review the options Claude surfaces. Install 2-3 skills that match your workflow. Good candidates:
- A shipping/deployment workflow
- A code review workflow
- A QA/testing workflow

For each skill you install, understand what it does and when you'd use it. Don't install things you won't use.

---

## Step 3: Create Custom Skills

This is where things get tailored to your project. Create at least two custom skills:

**Skill 1 -- Page scaffolding:**

> "Claude, create a custom skill called /new-page that creates a new page in our site following our existing patterns -- layout, styling, navigation link, and everything needed to match the current pages."

**Skill 2 -- Quality checks:**

> "Claude, create a custom skill called /check that runs lint, type-check, and build in sequence and reports any issues clearly."

**Skill 3 -- Deploy (if ready):**

> "Claude, create a custom skill called /deploy that commits all changes, pushes to main, and confirms the Vercel deployment."

After creating each skill, test it immediately. Invoke the skill and verify it does what you expect. If it doesn't, tell Claude to adjust it.

---

## Step 4: Set Up Hooks

Configure automatic quality gates:

**Hook 1 -- Auto-lint after file edits:**

> "Claude, configure a hook that runs the linter after every file edit."

**Hook 2 -- Type-check before commits:**

> "Claude, configure a hook that runs type-check before every commit."

After setting up each hook, trigger it intentionally. Edit a file and see if the linter runs. Try to commit and see if the type-check runs. Hooks that aren't tested are hooks that might not work.

---

## Step 5: Test the Whole Workflow

Now put it all together by adding a new page to your site.

**Use your /new-page skill:**

> "/new-page -- Create a 'Uses' page that lists the tools and technologies I use or recommend. Include sections for development tools, design tools, and productivity tools."

Watch what happens:
- The skill should scaffold the page following your existing patterns
- The linting hook should run automatically after files are edited
- The page should match the style and layout of your existing pages

**Run your /check skill:**

> "/check"

All quality gates should pass: lint, type-check, build. If anything fails, direct Claude to fix it.

**Deploy:**

> "/deploy"

Or if you created a /deploy skill:

Your changes should be committed, pushed, and deployed to Vercel.

**Verify the result:**

Visit your live site. Navigate to the new Uses page. Check that it matches the style of your other pages, the navigation includes it, and everything works.

---

## Step 6: Set Up Claude Chrome (If Available)

If you have the Chrome MCP tool configured:

> "Claude, use the Chrome tool to open my site and verify all pages load correctly and the navigation works."

> "Claude, check each page of my site in Chrome and report any visual issues or broken links."

This is a preview of the automated testing workflows you'll use in later weeks. If the Chrome tool isn't available, skip this step -- you'll set it up later in the curriculum.

---

## Acceptance Criteria

- [ ] CLAUDE.md is comprehensive -- covers stack, conventions, structure, and quality standards
- [ ] 2+ custom skills created and working (e.g., /new-page, /check, /deploy)
- [ ] At least 2 hooks configured (auto-lint after edits, type-check before commits)
- [ ] New "Uses" page added using the custom skill workflow
- [ ] All quality gates pass (lint, type-check, build)
- [ ] Site deployed with the new page live on Vercel

---

## Stretch Goals

If you finish early and want to go deeper:

**Create a /blog-post skill:**

> "Claude, create a skill called /blog-post that scaffolds a new blog entry with proper frontmatter, a template for the content, and adds it to the blog index page."

**Create a /lighthouse skill:**

> "Claude, create a skill called /lighthouse that runs a Lighthouse performance audit on my site and reports the scores for performance, accessibility, best practices, and SEO."

**Explore MCP servers:**

> "Claude, what MCP servers are available that could be useful for this project? Walk me through setting up one that adds a useful capability."

**Create a /setup-dev skill for future projects:**

> "Claude, create a skill called /setup-dev that initializes a new project with our standard stack: Next.js, TypeScript, Tailwind, shadcn/ui, ESLint, Prettier. This should be reusable for any new project."

---

## What You've Built

By completing this project, you now have:

1. A **comprehensive CLAUDE.md** that gives any Claude session full context about your project
2. **Custom skills** that encode your specific workflows so you never repeat yourself
3. **Automated hooks** that enforce quality standards without you thinking about it
4. A proven workflow: skill scaffolds the feature, hooks catch issues, quality checks verify everything, deploy ships it

This is your development environment for the rest of the curriculum. Every week from here benefits from what you set up this week.
