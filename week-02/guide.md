# Week 2 Guide: The Claude Code Workflow

This is the most important guide in the entire curriculum. The workflow you learn here is the one you will use for everything you build from now on.

---

## Part 1: The Research --> Plan --> Execute Model

This is THE workflow. Every feature, every page, every project follows this pattern. Internalize it.

### Step 1: Research — Use Plan Mode

Press **`Shift+Tab`** to enter **plan mode** before you start. In plan mode, Claude Code explores and thinks without changing any files. This is exactly what you want during research.

Ask Claude questions like:

- "What are the best practices for building an about page on a personal website?"
- "Show me examples of well-designed contact forms."
- "What tools or libraries are typically used for page transitions in Next.js?"

You are not wasting time here. The five minutes you spend researching save thirty minutes of building the wrong thing. And because you're in plan mode, Claude can't accidentally start building before you're ready.

### Step 2: Plan — Stay in Plan Mode

Still in plan mode. Now shift from "explore the problem" to "design the solution."

- "Plan the architecture for a multi-page personal website. What pages do we need? What components should be shared?"
- "Design the navigation structure and layout for this site."
- "What's the file structure going to look like for adding an about page?"

Review the plan. Ask questions. Push back on things that don't make sense. Adjust. Claude can revise the plan as many times as you want — nothing gets built until you say so.

This prevents the number one mistake beginners make: jumping straight to building without thinking. When you skip the plan, you end up with a mess that takes longer to fix than it would have taken to plan in the first place.

### Step 3: Execute — Switch to Normal Mode

Press **`Shift+Tab`** to switch to **normal mode**. Now Claude has permission to create and edit files. Time to build.

- Give clear, specific prompts about what to build.
- Review output frequently. Do not let Claude build 500 lines before you check what it made.
- Use quality gates (build, lint, type-check) after each significant change.

If you want more control during execution, press `Shift+Tab` again to enter **accept-edits mode** — Claude proposes each change and you approve it individually. This is great for sensitive parts of your project.

The cycle inside Execute is: **direct → review → adjust → repeat.**

### The Mode Map

| Phase | Mode | Toggle | Why |
|-------|------|--------|-----|
| Research | Plan mode | `Shift+Tab` | Claude explores without changing files |
| Plan | Plan mode | (stay) | Claude designs the solution, you review |
| Execute | Normal mode | `Shift+Tab` | Claude builds with full autonomy |
| Execute (careful) | Accept-edits mode | `Shift+Tab` | You approve each change individually |

This mode-switching habit is the single most important workflow skill in this course. It maps directly to how professionals use AI-native development: think first, build second, verify always.

---

## Part 2: CLAUDE.md -- Teaching Your AI About Your Project

### What It Is

CLAUDE.md is a file in your project root that Claude Code reads automatically at the start of every session. It is your project's memory.

### Why It Matters

Without CLAUDE.md, every new Claude Code session starts from zero. Claude has to re-discover your tech stack, your conventions, your preferences. With CLAUDE.md, Claude picks up right where you left off -- already knowing how your project works.

### What Goes In It

- **Project description:** What is this? What does it do? Who is it for?
- **Tech stack:** "This project uses Next.js App Router, TypeScript, Tailwind CSS, and shadcn/ui."
- **Conventions:** "Use server components by default. Only add 'use client' when the component needs interactivity." / "Use named exports, not default exports."
- **Quality standards:** "Always run lint and type-check after changes. All pages must be responsive."
- **File structure notes:** "Pages live in app/. Shared components live in components/. Styles use Tailwind utility classes."

### How to Create It

Just ask:

> "Claude, create a CLAUDE.md file for this project. Describe the tech stack, coding conventions, file structure, and quality standards based on what's already here."

Then read what Claude wrote. Edit it if something is wrong or missing. This file will evolve as your project grows.

### Mental Model

Think of CLAUDE.md as onboarding documentation for your AI team member. When a new developer joins a team, you hand them a doc that explains how the project works, what the conventions are, and what tools to use. CLAUDE.md is exactly that -- for Claude.

### Try It: /powerup

Run `/powerup` in Claude Code and complete this lesson:
- **"Teach Claude your rules"** -- Practice setting up CLAUDE.md and see how it changes Claude's behavior across sessions

---

## Part 3: How to Review Agent Output

You do not need to read code line-by-line. You are not a code reviewer. You are a quality inspector.

Here is your review checklist:

### Does it match what I asked for?

Open the browser. Click around. Does the behavior match your description? Does the page look like what you envisioned? If not, tell Claude what is wrong.

### Does it build?

Run `npm run build`. If it fails, copy the entire error and paste it back to Claude:

> "Claude, I got this error when running npm run build: [paste error]. Fix it."

A successful build means the code is structurally complete.

### Does it lint?

Run `npm run lint`. Linting catches sloppy patterns -- unused variables, inconsistent formatting, potential bugs. If it fails, paste the errors to Claude.

### Does it type-check?

If your project uses TypeScript, the type-checker catches a whole category of bugs before any user ever sees them. If `npm run build` passes (which includes type-checking in Next.js), you are in good shape.

### Does it look organized?

Open your editor and glance at the file structure. Are there a reasonable number of files? Are they in sensible directories? You do not need to read the files -- just check that the structure looks clean.

### The Bottom Line

Your quality gates are: **visual inspection, build, lint, type-check, file structure.** If all five pass, the code is solid. Move on.

---

## Part 4: Git Workflow with Claude Code

Git is your safety net. Learn these patterns and you will never lose work.

### The Golden Rule

**`main` is sacred.** Never build directly on main. Main is the live version of your site -- the one Vercel deploys. You do not experiment on the live version.

### The Branch Model

Every feature gets its own branch. A branch is a parallel copy of your project where you can make changes without affecting main.

**Starting a feature:**
> "Claude, create a new branch called feature/about-page and switch to it."

**While working:**
You make all your changes on the branch. Build, review, iterate -- all on the branch.

**When the feature is done:**
> "Claude, commit these changes with a descriptive message, push the branch, and create a pull request."

**Merging to main:**
A pull request is a checkpoint. It says "here are all the changes I want to add to main." You review them, then merge.

> "Claude, merge the pull request."

**After merging:**
> "Claude, switch back to main and pull the latest changes."

Now main includes your new feature, and you are ready to start the next one.

### Why This Matters

Branches let you experiment freely. If something goes wrong, main is untouched. You can throw away a branch and start over. You can have multiple features in progress at once. Branches are cheap -- use them for everything.

---

## Part 5: Prompt Engineering for Builders

The quality of what Claude builds depends directly on the quality of your direction. Here are the patterns that work.

### Be Specific

Bad: "Add a form."

Good: "Add a contact page with a form that has fields for name, email, and message. Include a submit button. Use Tailwind for styling."

The more specific you are, the less Claude has to guess.

### Include Constraints

> "Use Tailwind for styling. Keep it consistent with the existing pages. Make sure it's responsive."

Constraints prevent Claude from making choices that don't fit your project.

### Reference Existing Work

> "Style the about page to match the homepage layout -- same max-width, same font sizes, same spacing."

Claude can look at your existing files and replicate patterns. Use this.

### Ask for Options

> "Give me 3 different layout options for the projects page. Describe each one before building."

This is the plan step in miniature. See options before committing to one.

### Iterate

> "The spacing is too tight. Add more padding between sections."
> "Make the heading larger and bolder."
> "Move the contact form to the left side of the page."

Small, focused adjustments are better than trying to describe everything perfectly upfront.

### Correct Course

> "That's not what I meant. I want a grid of project cards, not a list. Each card should have an image, title, and short description."

If Claude misunderstands, do not try to salvage bad output. Tell it clearly what you actually want.

---

## Part 6: When Things Go Wrong

Things will go wrong. This is normal. Here is how to handle every situation.

### Error Messages Are Information, Not Failures

When you see an error, do not panic. Copy the entire error message and hand it to Claude:

> "Claude, I got this error: [paste the full error]. Fix it."

Claude is excellent at interpreting error messages. Nine times out of ten, it will fix the issue on the first try.

### When Claude Goes in Circles

Sometimes Claude will try to fix something, make it worse, try again, make it worse again. If you notice this loop, break out of it:

> "Stop. Let's think about this differently. The problem is [describe what's actually wrong in plain English]. What's the right approach?"

Resetting the framing often breaks the cycle.

### When the Whole Project Feels Broken

Git is your safety net. If things are really messed up:

> "Claude, show me the recent git log."

Find the last commit where things worked, then:

> "Claude, revert to commit [hash]."

You are back to a working state. Try again with a different approach.

### The Nuclear Option

If Claude's context is confused and nothing is making sense, start a new Claude Code session. Close the terminal, open a new one, and start fresh. Claude will re-read your CLAUDE.md and start with clean context.

This is not a failure. It is a tool in your toolkit.

### Try It: /powerup

Run `/powerup` in Claude Code and complete these lessons:
- **"Undo anything"** -- Learn `/rewind` and `Esc-Esc` for quick recovery when things go sideways
- **"Run in the background"** -- Learn to run tasks in the background so you can keep working while Claude executes
