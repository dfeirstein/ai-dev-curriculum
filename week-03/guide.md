# Week 3 Guide: Skills, Hooks, and Power Tools

---

## Part 1: Claude Code Is Customizable

Out of the box, Claude Code is powerful. You've already used it to build and deploy a multi-page website. But what you've been using is the *default* Claude Code -- the general-purpose version that works the same for everyone.

This week, you make it **yours**.

There are four customization layers, and they stack on top of each other:

1. **CLAUDE.md** -- You already know this one. It gives Claude persistent context about your project: what stack you use, what conventions to follow, what the project structure looks like.

2. **Skills** -- Packaged instructions that teach Claude specific workflows. Think of them as specialists you can call on. A `/deploy` skill knows your exact deploy process. A `/qa` skill knows how to test your app. You can install community skills or create your own.

3. **Hooks** -- Commands that run automatically before or after Claude takes certain actions. After every file edit, run the linter. Before every commit, run type-check. You set them up once and they work forever.

4. **MCP Servers** -- Connections to external tools and services. They let Claude interact with things outside your codebase: browsers, databases, APIs, design tools.

All of this customization lives in one place: the **`.claude/` directory** inside your project. Think of it as Claude Code's profile for your project. When you open a project that has a `.claude/` directory, Claude Code automatically loads all your customizations.

The result: instead of starting from scratch every time, Claude Code already knows your conventions, has your workflows ready, and enforces your quality standards automatically.

---

## Part 2: Skills -- Extending What Claude Can Do

### What is a skill?

A skill is a packaged set of instructions that teaches Claude Code a specific workflow. When you invoke a skill, Claude follows those instructions instead of figuring things out from scratch.

The analogy: imagine you run a restaurant. You *could* explain your plating style, portion sizes, and presentation standards every single time a new dish is prepared. Or you could write it down once, and every cook follows the same playbook. Skills are the playbook.

### Why skills matter

Without skills, you repeat yourself. Every time you want to add a new page to your site, you tell Claude the same things: follow the existing layout, add a nav link, use the same styling approach, make sure it matches the other pages. With a `/new-page` skill, all of that is encoded once. You just say "use /new-page to create a Contact page" and Claude knows exactly what to do.

### Installing community skills

The Claude Code ecosystem includes skills built by other developers. These are pre-packaged workflows you can install and use immediately. To explore what's available:

> "Claude, what skills are available that would help with a Next.js project?"

Some examples of what community skills can do:
- **/ship** -- Commit changes, create a pull request, and deploy in one flow
- **/review** -- Run a structured code review against best practices
- **/qa** -- Systematically test your application and report issues

You don't need to understand how the skill works internally. You need to understand what it does and when to use it.

### Creating your own skills

Community skills are general-purpose. Your *own* skills are tailored to your exact project and workflow. You should create a custom skill whenever you find yourself giving Claude the same instructions more than twice.

**What a skill contains:**
- **A trigger** -- The command that activates it (e.g., `/new-page`, `/check`, `/deploy`)
- **Instructions** -- What Claude should do when the skill is invoked
- **Tool access** -- What tools Claude can use during the workflow

**You don't write skill files by hand.** You direct Claude Code to create them for you.

**Example -- a page scaffolding skill:**

> "Claude, create a custom skill called /new-page that creates a new page in our site following our existing patterns -- layout, styling, navigation link, and everything needed to match the current pages."

Claude Code will create the skill definition in your `.claude/commands/` directory. From that point on, you can invoke `/new-page` and Claude will follow the encoded instructions.

**Example -- a quality check skill:**

> "Claude, create a custom skill called /check that runs lint, type-check, and build in sequence and reports any issues clearly."

Now instead of remembering to run three separate checks, you run `/check` and get a clean report.

**Example -- a deploy skill:**

> "Claude, create a custom skill called /deploy that commits all changes, pushes to main, and confirms the Vercel deployment."

**Example -- a project setup skill:**

> "Claude, create a custom skill called /setup-dev that initializes a new project with our standard stack: Next.js, TypeScript, Tailwind, shadcn/ui, ESLint, Prettier."

This last one is especially powerful. You define your ideal project setup once, and every future project starts from the same solid foundation.

### The compounding effect

Each skill you create makes the next project faster. Over time, you build a library of workflows that represent *your* way of building software. That library is portable -- it travels with you from project to project.

---

## Part 3: Hooks -- Automation That Runs Itself

### What are hooks?

Hooks are commands that run automatically before or after Claude Code takes certain actions. You don't invoke them. They just happen.

### Why hooks matter

Quality gates should be automatic. Every developer knows they *should* run the linter after editing files. Every developer knows they *should* type-check before committing. But in practice, people forget. They get focused on the feature and skip the checks.

Hooks remove the "remembering" part. You set them up once, and they enforce your standards every time, without you thinking about it.

### The mental model

Think of hooks as a checklist that runs itself. A pilot doesn't decide whether to run pre-flight checks -- the checklist is mandatory and built into the process. Hooks are your pre-flight checklist for code.

### Examples of useful hooks

**After every file edit -- auto-run the linter:**
When Claude Code edits any file, the linter runs immediately. If there's a formatting issue or style violation, it gets caught right away -- not ten edits later when you've lost the context.

**Before every commit -- run type-check and build:**
Before changes are committed to Git, the type-checker and build process run. If there's a type error or build failure, the commit is blocked. Broken code never makes it into your Git history.

**After creating a PR -- run tests:**
When a pull request is created, automated tests run. You get immediate feedback on whether the changes work.

### How to set them up

You direct Claude Code to configure hooks in your project settings:

> "Claude, configure a hook that runs the linter after every file edit."

> "Claude, configure a hook that runs type-check before every commit."

Claude Code will update the `settings.json` file in your `.claude/` directory. The hooks are active immediately.

### Hooks vs. skills

Skills are things you invoke on purpose: "Run /check." Hooks are things that happen automatically: the linter runs after every edit whether you asked for it or not.

They complement each other. Skills give you on-demand workflows. Hooks give you always-on quality gates.

---

## Part 4: MCP Servers -- Connecting to External Tools

### What are MCP servers?

MCP stands for Model Context Protocol. In practical terms, MCP servers give Claude Code the ability to interact with external services and tools that exist outside your codebase.

By default, Claude Code can read and write files, run terminal commands, and search your code. MCP servers extend that reach to things like web browsers, databases, design tools, and APIs.

### Claude Chrome tool

One of the most useful MCP connections is the Chrome browser tool. It lets Claude Code control a web browser -- navigating to URLs, clicking buttons, filling forms, and reading page content.

Why is this useful?

- **Visual verification:** "Claude, open the site in Chrome and check if the navigation works on mobile."
- **Automated testing:** "Claude, go to each page of my site and verify they all load without errors."
- **Interacting with web services:** Claude can navigate to external tools and services on your behalf.

You don't need to understand the protocol. You just need to know that when Claude has access to Chrome, you can ask it to do things in a browser.

### Database MCP

If your project has a database (you'll add one in later weeks), a database MCP server lets Claude Code introspect your database directly -- examining tables, running queries, and understanding your data structure without you having to explain it.

### Custom MCP servers

For specialized tools and services, custom MCP servers can be built or installed. These might connect Claude to your company's internal tools, specific APIs, or proprietary services.

### The key takeaway

MCP servers expand what Claude Code can *do*. Without them, Claude works with files and terminal commands. With them, Claude can browse the web, query databases, generate images, and interact with any service that has an MCP server.

You don't need to understand how MCP works internally. You need to know what capabilities it unlocks and when to reach for them.

---

## Part 5: The .claude/ Directory

All of your Claude Code customization lives in one place: the `.claude/` directory at the root of your project. Here's what's inside:

### settings.json

Your project-level settings for Claude Code. This is where hooks live, along with permissions and preferences. When you ask Claude to "configure a hook," this is the file that gets updated.

### commands/

This is where your custom skills are stored. Each skill is a file in this directory. When you ask Claude to "create a custom skill called /new-page," it creates a file here.

### memory/

Persistent context that Claude remembers across sessions. While CLAUDE.md is the primary way to give Claude project context, the memory system handles things Claude learns during your work sessions.

### Think of it as a profile

Your `.claude/` directory is Claude Code's profile for your project. It tells Claude:
- What quality standards to enforce (hooks)
- What workflows to follow (skills)
- What permissions it has (settings)
- What it has learned about your project (memory)

This directory should be committed to Git. That means when you clone your project on a different machine, or when a collaborator clones it, all your Claude Code customizations come along for the ride.

---

## Part 6: The Power User Mindset

There are two ways to use Claude Code.

### The basic approach

You open Claude Code and say: "Build me a contact form." Claude builds something. Maybe it matches your style, maybe it doesn't. You give follow-up instructions to fix things. You manually run the linter. You manually check types. You manually deploy.

It works. But it's slow, and it doesn't get better over time.

### The power user approach

You invest time upfront:

- Your **CLAUDE.md** describes your project's conventions, stack, structure, and quality standards in detail. Claude doesn't have to guess -- it knows exactly how your project works.
- Your **skills** encode your common workflows. `/new-page` scaffolds a page that matches your existing patterns. `/check` runs all quality gates. `/deploy` ships to production. You don't repeat yourself.
- Your **hooks** enforce quality automatically. Every file edit gets linted. Every commit gets type-checked. Broken code never slips through.

Now when you say "Add a contact page using our standard patterns," Claude knows what "our standard patterns" means (CLAUDE.md), follows the right workflow (skills), and quality is checked automatically (hooks).

### The investment compounds

The first time you set up skills and hooks, it takes time. But every project after that benefits. Your `/setup-dev` skill initializes new projects with your preferred stack. Your `/check` skill works in every project. Your hooks enforce quality everywhere.

The gap between a basic user and a power user widens with every project. After ten projects, the power user has a library of workflows, a refined CLAUDE.md, and a development environment that practically runs itself. The basic user is still typing the same instructions from scratch.

This week, you become a power user.
