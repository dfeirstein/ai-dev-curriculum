# Week 1 Guide: Your AI Development Environment

---

## Part 1: The Big Picture

### What Is Software Development in 2026?

Here's the short version:

1. **You describe what you want.**
2. **An AI builds it.**
3. **You verify it works.**
4. **You ship it.**

That's it. That's the job now.

Not long ago, building software meant spending years learning programming languages, memorizing syntax, debugging semicolons, and reading thousands of lines of other people's code. Some people still work that way, and they're very good at it. But that's no longer the only path -- and for many projects, it's not even the best path.

Today, the most powerful skill in software development isn't knowing how to write code. It's knowing **what to build**, **how to describe it clearly**, and **how to tell if it actually works**.

### The Old Way vs. The New Way

**Traditional developers** spend roughly 80% of their time reading and writing code. They think in syntax, functions, and data structures. They're like camera operators -- technically skilled, deeply focused on the mechanics of the craft.

**AI-native builders** spend 80% of their time thinking about **what to build** and **verifying it works**. They describe their vision, review the output, give feedback, and make decisions. They're like film directors -- they don't operate the camera themselves, but they absolutely control what ends up on screen.

You are learning to be a director.

### Your Role

As an AI-native builder, you wear four hats:

- **Researcher:** You figure out what needs to exist. What problem are you solving? Who is it for? What should it look like and feel like?
- **Architect:** You make high-level decisions. What tools should this use? How should it be organized? What matters most?
- **Quality Inspector:** You verify the work. Does it run? Does it look right? Does it do what you asked? You don't need to understand every line of code -- you need to know when something is wrong.
- **Shipping Captain:** You get it live. Code sitting on your laptop helps nobody. Your job is to get it out into the world where people can use it.

---

## Part 2: Your Tools

You have six tools in your environment. Here's what each one does and why it matters.

### Ghostty -- Your Terminal

Ghostty is a terminal application. A terminal is a text-based interface to your computer. Instead of clicking buttons and icons, you type commands.

Why text? Because Claude Code lives in the terminal. When you talk to Claude Code, you're typing in Ghostty. When Claude Code builds things for you, the output appears in Ghostty. It's your command center.

You don't need to become a terminal expert. You need to be comfortable opening it, typing a few things, and reading what comes back.

### Zed -- Your Editor

Zed is a code editor. It shows you the files that Claude Code creates and modifies.

Here's the key insight: **you'll rarely type in Zed.** You're not here to write code. You're here to review what Claude Code built. Open Zed, browse the files, get a general sense of what's there. Think of it like reviewing a blueprint -- you don't need to understand every measurement, but you should be able to tell if it's a house or a garage.

To open your current project in Zed, type `zed .` in your terminal.

### Claude Code CLI -- Your AI Builder

This is the core of everything. Claude Code is an AI that lives in your terminal. You describe what you want in plain English, and it writes the code, creates the files, and sets things up.

You start it by typing `claude` in Ghostty. From that moment, you're in a conversation. You talk, it builds. You give feedback, it adjusts. This back-and-forth is how all your software gets made.

Claude Code isn't magic -- it's a very capable collaborator. The better you describe what you want, the better the results. Vague asks get vague results. Specific asks get specific results. You'll get better at this with practice.

### Git -- Your Save Points

Git tracks every change you make to your project. Think of it like a video game save system. Every time you reach a good state, you save. If you mess something up later, you can go back to any previous save.

Each save is called a **commit**. Each commit has a short message describing what changed. Over time, you build up a history of your entire project, step by step.

You won't use Git directly. You'll tell Claude Code to handle it for you. But understanding what it does helps you know why it matters.

### GitHub -- Your Code on the Internet

GitHub is where your code lives online. It's like cloud storage for your projects, but with superpowers: version history, collaboration tools, and the ability to connect to other services (like deployment).

When you "push" your code to GitHub, you're sending your local project -- along with all its save points -- up to the cloud. Now it's backed up, shareable, and ready to deploy.

### GitHub CLI (gh) -- GitHub from Your Terminal

The GitHub CLI lets you do GitHub things without opening a web browser. Create repositories, check on pull requests, manage issues -- all from your terminal. Claude Code uses it behind the scenes when you ask it to set up a repo or push your code.

### Vercel -- Your Deploy Button

Vercel takes your code and puts it on the internet. It connects to your GitHub repository, and every time you push new code, Vercel automatically builds and deploys your site. Within seconds, your changes are live at a URL anyone in the world can visit.

That's the pipeline: you direct Claude Code to build something, commit it to Git, push it to GitHub, and Vercel deploys it. You go from idea to live website without ever writing a line of code yourself.

---

## Part 3: Your First Session with Claude Code

### Starting Up

Open Ghostty. Type:

```
claude
```

That's it. You're now in a conversation with your AI builder. Everything you type from here is a prompt -- a description of what you want Claude Code to do.

### The Interaction Model

You describe what you want in plain English. Claude Code builds it. You review the result. If it's not right, you describe what to change. Repeat until it's good.

This is a conversation, not a command line. You can be natural. You can be specific. You can change your mind. The key mental shift is this: **focus on WHAT you want, not HOW to code it.**

You never need to say "write a function that maps over an array and returns..." You say "show me a list of my recent projects with their titles and dates."

### Example Prompts

Here are the kinds of things you'll say to Claude Code. Notice -- these are descriptions of outcomes, not technical instructions:

> "Create a new Next.js project called my-site with TypeScript, Tailwind CSS, and the App Router"

> "Change the homepage to show my name, a one-line bio, and links to my GitHub and LinkedIn"

> "Make it look professional -- clean typography, centered layout, good spacing"

> "The heading is too big and the links should be on one line instead of stacked vertically"

> "Add a subtle gradient background, something modern and not too flashy"

See the pattern? You're describing what you want to see. You're giving creative direction. You're being specific about outcomes.

### Reviewing What Claude Code Built

After Claude Code makes changes, you'll want to see what happened. Two ways:

1. **In the browser:** If you have a dev server running, just refresh the page. You'll see the visual result immediately.
2. **In Zed:** Type `zed .` to open your project files. Browse around. You don't need to understand every line. Ask yourself: "Does this look reasonable? Does the structure make sense? Do I see the things I asked for?"

If you're curious about something Claude Code created, just ask it: "Explain what this file does" or "Why did you create this component?"

### The Feedback Loop

Your first result won't always be perfect. That's normal and expected. The key is giving **specific feedback**:

- **Good:** "The text is too small on mobile. Make the heading larger and add more padding on the sides."
- **Not helpful:** "Fix it."
- **Good:** "I want the background to be dark with light text, and the links should be blue."
- **Not helpful:** "Make it look better."

The more precisely you describe what's wrong and what you want instead, the faster Claude Code gets you there. This is a skill, and you'll improve at it every week.

---

## Part 4: Git -- Your Safety Net

### The Concept

Imagine you're painting a canvas. Every so often, you take a photograph of your work. If you make a mistake, you can look at any previous photograph and restore the painting to that state.

Git does this for your code. Every time you commit, you're taking a snapshot. Your entire project history is preserved. Nothing is ever truly lost.

### How You'll Use It

You won't type Git commands yourself. You'll direct Claude Code:

> "Initialize a git repo, commit everything, create a GitHub repo, and push"

Claude Code handles the details. But it helps to understand what's happening behind the scenes:

- **`git init`** -- Start tracking this project. This creates a hidden folder that stores all your snapshots.
- **`git add .`** -- Stage all current changes. This is like saying "these are the changes I want in my next snapshot."
- **`git commit -m "message"`** -- Save a snapshot with a description. The message should say what changed, like "Add landing page with bio and links."
- **`git push`** -- Send everything to GitHub. Now your code is backed up in the cloud and ready for deployment.
- **`gh repo create`** -- Create a new repository on GitHub. This is the online home for your project.

### Useful Things to Ask Claude Code

- "What changed since my last commit?" -- See what's been modified since your last save point.
- "Undo the last change" -- Go back one step.
- "Show me the commit history" -- See all your snapshots in order.
- "Commit what we have so far with a good message" -- Save your current state.

Think of commits as a timeline of your project. Good, frequent commits mean you always have a safe state to return to.

---

## Part 5: Deploying -- Your Code on the Internet

### The Concept

Your code on your laptop is like a painting in your closet. Deployment is hanging it in a gallery. It takes your project from "works on my machine" to "anyone with the URL can see it."

**Deployment** means your code is running on a server somewhere on the internet, accessible via a URL. When someone types that URL into their browser, they see your site.

### How Vercel Works

Vercel connects to your GitHub repository. Here's the flow:

1. You push code to GitHub.
2. Vercel detects the new code.
3. Vercel builds your project automatically.
4. Your site is live at a URL (something like `your-project.vercel.app`).

Every time you push new code, Vercel rebuilds and redeploys. This means updating your live site is as simple as pushing a new commit.

### Directing Claude Code to Deploy

When you're ready to go live, tell Claude Code:

> "Deploy this to Vercel"

or

> "Connect this repo to Vercel and deploy it"

Claude Code will walk you through the process. If there are any setup steps (like linking your Vercel account), it will guide you.

### The Magic Moment

When you visit your deployed URL and see your site live on the internet -- that's real. You built that. Not by writing code line by line, but by directing an AI to build it for you, verifying it worked, and shipping it. That's the skill set that matters now.

Share the URL with a friend, a family member, anyone. Watch their reaction when they see something you made, live on the web.

---

## Part 6: Quality Gates

### Your Job Is to Verify

Claude Code builds. You verify. This is a responsibility, not a formality. Shipping broken software is worse than not shipping at all.

After Claude Code makes changes, run through these checks:

### Check 1: Does It Build?

Ask Claude Code to run the build:

> "Run npm run build and show me if there are any errors"

If the build passes, your code compiles correctly. If it fails, you'll see error messages. You don't need to understand the errors yourself. Just copy them and paste them back to Claude Code:

> "I got this error when running npm run build: [paste the error here]. Fix it."

### Check 2: Does It Look Right?

Open your site in the browser (usually at `localhost:3000` during development). Look at it. Actually look at it. Ask yourself:

- Does it show what I asked for?
- Does the layout look clean?
- Is the text readable?
- Do the links work?
- Does it look good on different screen sizes? (Resize your browser window to check.)

If something looks off, describe it to Claude Code. Be specific about what's wrong and what you want instead.

### Check 3: Does It Work?

Click every link. Try every interaction. If there's a button, press it. If there's a link, follow it. Make sure everything does what it's supposed to do.

### The Pattern

This is the quality pattern you'll use in every project going forward:

1. Claude Code makes changes.
2. You build the project to check for errors.
3. You look at it in the browser to check visually.
4. You interact with it to check functionality.
5. If anything fails, you tell Claude Code what's wrong and it fixes it.

You are the quality gate. Code doesn't ship until you say it's ready.

---

## What's Next

You've got the concepts. Now go build something. Head to [project.md](project.md) for the step-by-step project: your personal landing page, live on the internet.
