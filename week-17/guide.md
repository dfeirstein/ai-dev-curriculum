# Week 17 Guide: Advanced AI-Native Patterns

---

## Part 1: Complex Prompting Patterns

### Beyond Single Prompts

For the first 16 weeks, most of your prompts have been straightforward: "Build this feature," "Fix this bug," "Add this test." That works for well-defined tasks. But as the problems get more complex, single prompts aren't enough.

Advanced AI-native building uses **prompt chains** -- sequences of prompts where each one builds on the output of the last. The key insight: you're not just asking Claude Code to do things. You're guiding it through a thinking process.

### The Analyze-Plan-Execute Pattern

This is the most powerful pattern in your toolkit. Instead of jumping straight to implementation, you break the work into three distinct prompts:

**Step 1 -- Analyze:**
> "Analyze the current authentication flow in this project. How does it work? What are the entry points? Where are the edge cases? Don't change anything -- just explain what's happening."

**Step 2 -- Plan:**
> "Based on that analysis, plan how we'd add social login (Google and GitHub). What needs to change? What can stay the same? What are the risks? Give me the plan before you implement anything."

**Step 3 -- Execute:**
> "Implement the plan. Start with [the first step] and let me review before moving to the next."

Why this works: Claude Code gives better results when it understands the context before acting. The analyze step builds that context. The plan step lets you catch wrong directions before any code is written. The execute step is surgical because the planning is done.

### The Incremental Refactoring Pattern

Large refactors are where AI makes the most mistakes. Changing too many things at once leads to cascading failures. The solution: incremental prompts.

> "Step 1: Identify all the places in this project where we directly query the database outside of a dedicated data access function."

> "Step 2: Create a data access layer -- a set of functions in a `/lib/data` folder that encapsulate each database query. Don't change any callers yet."

> "Step 3: Now update [specific file] to use the new data access functions instead of direct queries. Only this file -- nothing else."

> "Step 4: Run the tests. Did anything break?"

Each step is small, verifiable, and reversible. If step 3 breaks something, you only need to undo one file's changes, not a massive refactor.

### The Comparison Pattern

When you're making architecture decisions, ask Claude Code to compare options:

> "I need to add real-time updates to this app. Compare three approaches: polling, Server-Sent Events, and WebSockets. For each, explain: how it works, the pros and cons, and how complex it would be to implement in this specific project. Recommend one and explain why."

This turns Claude Code into a research assistant. You get a structured comparison without having to read three separate documentation sites.

---

## Part 2: When Claude Gets It Wrong

### Recognizing Bad Output

Claude Code is not always right. After 16 weeks, you have enough experience to recognize when something is off. Here are the most common failure modes:

**Over-engineering:** Claude Code sometimes builds more than you asked for. You wanted a simple function; it gave you a complex class hierarchy. Solution: "This is too complex. Simplify it. I just need [specific thing]."

**Wrong assumptions:** Claude Code may assume things about your project that aren't true. It might use a library you don't have installed, or assume a database table that doesn't exist. Solution: "That references [thing that doesn't exist]. Look at the actual project structure and try again."

**Outdated patterns:** AI models have training cutoffs. Claude Code might suggest patterns that were best practice two years ago but have been superseded. Solution: "Is this the current recommended approach for [framework/tool]? Check against the latest patterns."

**Confident but wrong:** Claude Code presents everything with confidence, even when it's wrong. Just because it sounds certain doesn't mean it's correct. Always verify. Solution: test the output, check the results, and trust your eyes over Claude Code's words.

### Course Correction Techniques

When Claude Code goes in the wrong direction:

1. **Be specific about what's wrong.** Don't say "this doesn't work." Say "this fails because [specific reason]."
2. **Show the error.** Copy and paste the actual error message or describe the exact wrong behavior.
3. **Constrain the solution.** "Fix this, but don't change [file/function]. Only modify [specific area]."
4. **Start fresh when needed.** Sometimes the conversation has gone too far in the wrong direction. It's faster to start a new Claude Code session with a clean prompt than to keep correcting.

---

## Part 3: Architecture Reviews

### Using Claude Code as a Reviewer

One of the most underused capabilities of Claude Code is review. You can ask it to review your own code with a critical eye:

> "Review the architecture of this project. Look at the folder structure, data flow, error handling, and separation of concerns. What's working well? What would you change? Be specific and critical."

> "Review this project for security issues. Check for: exposed secrets, SQL injection risks, missing input validation, improper auth checks, and any data that's accessible without proper authorization."

> "Review the database schema. Are there missing indexes? Redundant columns? Relationships that should exist but don't? Suggest improvements."

These reviews aren't a replacement for real security audits or professional code review. But they catch a surprising number of real issues, and they teach you what to look for.

### PR Review Patterns

When you're working on a team -- or reviewing your own pull requests before merging -- Claude Code can help:

> "Review this git diff. Look for: bugs, edge cases that aren't handled, performance issues, and anything that doesn't match the patterns used elsewhere in this project."

> "I'm about to merge this PR. Give me a checklist of things to verify before I merge."

---

## Part 4: Reading Unfamiliar Codebases

### The Exploration Workflow

The ability to understand code you didn't write is essential for open source contribution, joining teams, and learning from other projects. Claude Code makes this dramatically easier.

**Start broad:**
> "I just cloned the [project name] repository. Give me a high-level overview: what does this project do, how is it organized, and what are the key files and directories?"

**Go deeper on what interests you:**
> "Explain how the authentication system works in this project. Walk me through the flow from a user clicking 'sign in' to getting a session."

**Understand specific code:**
> "Explain what this function does, why it's written this way, and what would happen if we changed [specific thing]."

**Find connection points:**
> "How would I add a new feature that [description]? What files would I need to modify and what's the pattern I should follow?"

This workflow lets you understand any codebase in hours instead of days. You don't need to read every file. You need to understand the structure, the patterns, and the specific areas relevant to your work.

---

## Part 5: Contributing to Open Source

### Why Open Source Matters

Every tool in your stack -- Next.js, Drizzle, Better Auth, shadcn/ui, Tailwind -- is open source. Real people built them. And they all have issues that need fixing, documentation that needs improving, and features that need building.

Contributing to open source is:
- **Proof of skill.** A merged PR to a popular project is more impressive than any tutorial.
- **Real-world experience.** Open source codebases are production code with real users.
- **Networking.** Maintainers remember good contributors.
- **Giving back.** You've used these tools for free. Improving them helps everyone.

### The Contribution Workflow

1. **Pick a project.** Choose something you've used and understand conceptually. Better Auth, Drizzle, shadcn/ui, and similar projects are good candidates because you've worked with them.

2. **Find a good issue.** Look for issues labeled "good first issue," "help wanted," or "documentation." These are explicitly marked as approachable for new contributors.

3. **Understand the codebase.** Clone it. Use Claude Code to explore it (the patterns from Part 4). Understand how it's organized, how to run tests, and what the contribution guidelines say.

4. **Implement your contribution.** Use Claude Code to make the changes. Follow the project's patterns and conventions.

5. **Test thoroughly.** Run the project's test suite. Add tests for your changes if applicable.

6. **Submit a PR.** Write a clear description of what you changed and why. Reference the issue you're addressing.

### Not Just Code

Open source contributions don't have to be code. Documentation improvements, bug reports with clear reproduction steps, and example projects are all valuable contributions. If you find confusing documentation in a tool you've used, fixing it is a legitimate and appreciated contribution.

---

## Part 6: Publishing npm Packages

### Extracting Reusable Code

If you choose Option B, you'll extract something reusable from your projects and publish it as an npm package. This is how the tools you use every day were built -- someone extracted a useful pattern from their project and shared it.

Look through your projects for code that:
- Solves a general problem (not specific to your app)
- Could be useful to other developers
- Is well-contained and doesn't depend on your specific setup

Examples: a custom React hook, a validation utility, a UI component, a data formatting library, an auth helper.

### The Publishing Workflow

1. **Identify the code** to extract.
2. **Create a new project** with proper package structure.
3. **Extract and generalize** the code (remove app-specific references).
4. **Add documentation** (README with usage examples).
5. **Add tests** (comprehensive, not just happy path).
6. **Publish to npm.**

You'll direct Claude Code through every step. The key skill here isn't the mechanics of npm publishing -- it's the judgment of what's worth extracting and how to make it useful to others.
