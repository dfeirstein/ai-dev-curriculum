# Project: Advanced AI-Native Patterns

**What you're building:** Either a meaningful open source contribution (Option A) or a published npm package (Option B).

**Time estimate:** 12-15 hours across the week

**What you'll need open:** Ghostty, your browser, GitHub

---

## Choose Your Path

### Option A: Open Source Contribution

Contribute to a project you've used in this curriculum. Understand its codebase, find a good issue, implement a fix or feature, and submit a pull request.

### Option B: npm Package

Extract a reusable piece from one of your projects, package it properly, add documentation and tests, and publish it to npm.

Both options are equally valid. Pick the one that excites you more.

---

## Option A: Open Source Contribution

### Step 1: Choose a Project (Day 1)

Pick a project you've used and understand conceptually. Good candidates:

- **[Better Auth](https://github.com/better-auth/better-auth)** -- The auth library you used in TeamTask Pro
- **[Drizzle ORM](https://github.com/drizzle-team/drizzle-orm)** -- The database ORM from your stack
- **[shadcn/ui](https://github.com/shadcn-ui/ui)** -- The component library you've used throughout
- **[Zod](https://github.com/colinhacks/zod)** -- The validation library
- **Any other open source project** you've used and found useful

### Step 2: Explore the Codebase (Day 1-2)

Clone the repository and use Claude Code to understand it:

> "I just cloned the [project name] repository. Give me a high-level overview of the project structure, the key directories, and how the codebase is organized."

> "Explain the contribution guidelines. What do I need to do to submit a PR? Are there specific testing requirements or code style rules?"

> "Walk me through how [specific feature you've used] is implemented. What files are involved and how do they connect?"

### Step 3: Find an Issue (Day 2)

Browse the project's GitHub issues. Look for labels like:
- `good first issue`
- `help wanted`
- `documentation`
- `bug`

When you find a promising issue, use Claude Code to evaluate it:

> "Here's a GitHub issue for [project name]: [paste the issue description]. Based on your understanding of this codebase, how difficult is this? What files would need to change? Is this a reasonable contribution for someone new to the project?"

If an issue is too complex, find another one. If no labeled issues look approachable, consider:
- Fixing a typo or improving unclear documentation
- Adding a missing test case
- Improving an error message
- Adding a usage example to the docs

### Step 4: Implement the Contribution (Day 3-4)

Create a branch for your work:

> "Create a new branch called 'fix/[issue-description]' for this contribution."

Implement the changes using Claude Code. Follow the project's patterns:

> "Implement a fix for this issue: [description]. Follow the existing code patterns in this project. Match the code style, naming conventions, and file organization that's already in use."

### Step 5: Test Your Changes (Day 4)

> "Run the project's test suite. Do all existing tests still pass?"

> "Write tests for the changes I made, following the testing patterns used in this project."

> "Are there any edge cases in my changes that aren't covered by tests?"

### Step 6: Submit the PR (Day 5)

> "Help me write a clear PR description for this contribution. Include: what the issue was, what I changed, how I tested it, and any notes for the reviewer."

Submit the PR on GitHub. Be responsive to feedback from maintainers -- they may request changes, which is normal and expected.

### Acceptance Criteria (Option A)

- [ ] **Chose an open source project** you've actually used
- [ ] **Explored and understood** the relevant parts of the codebase using Claude Code
- [ ] **Found a real issue** to address (code fix, documentation improvement, or new test)
- [ ] **Implemented the contribution** following the project's conventions
- [ ] **All existing tests pass** and new tests are added where appropriate
- [ ] **Submitted a PR** with a clear, professional description
- [ ] **Wrote a comprehensive README** documenting your contribution process

---

## Option B: npm Package

### Step 1: Identify What to Extract (Day 1)

Review your projects -- TeamTask Pro, your independent project, your portfolio. Look for code that:

- Solves a general problem (not tied to your specific app)
- Could save other developers time
- Is well-defined with clear inputs and outputs

Examples of what you might extract:
- A custom React hook (useLocalStorage, useDebounce, useMediaQuery)
- A set of validation schemas (Zod schemas for common patterns)
- A UI component with specific behavior (a toast notification system, a data table)
- A utility library (date formatting, string manipulation, data transformation)
- An auth helper or middleware pattern

Ask Claude Code for suggestions:

> "Review the codebase of [your project]. What pieces of code are general-purpose enough to be useful as a standalone npm package? List candidates with a brief description of what each does."

### Step 2: Create the Package Project (Day 1-2)

> "Create a new npm package project called [your-package-name]. Set up: TypeScript configuration, ESLint, Vitest for testing, a proper package.json with the right fields (name, version, description, main, types, keywords, license), and a build step that outputs both CommonJS and ESM."

### Step 3: Extract and Generalize the Code (Day 2-3)

> "Extract the [code you identified] from [your project] into this new package. Remove any app-specific references. Make it configurable where appropriate. Add TypeScript types for all public APIs."

Review the extracted code:

> "Is this code truly general-purpose now? Could someone use this in any Next.js or React project without modification? What assumptions are baked in that shouldn't be?"

### Step 4: Write Documentation (Day 3-4)

> "Write a comprehensive README for this package. Include:
> - A clear description of what it does and why someone would use it
> - Installation instructions
> - Usage examples covering the main use cases
> - API reference documenting all exported functions/components and their parameters
> - TypeScript types documentation
> - A 'Contributing' section for anyone who wants to improve the package"

Good documentation is the difference between a package that gets used and one that gets ignored. Your README is your package's landing page.

### Step 5: Write Comprehensive Tests (Day 4)

> "Write comprehensive tests for this package. Cover:
> - All public API functions and their parameters
> - Edge cases (empty inputs, invalid data, boundary values)
> - TypeScript type correctness
> - Error conditions and error messages
> - Integration between functions if applicable
>
> Aim for at least 20 meaningful test cases."

Run the tests and make sure everything passes.

### Step 6: Publish to npm (Day 5)

First, create an npm account at npmjs.com if you don't have one.

> "Walk me through publishing this package to npm. Make sure the package.json is complete with all required fields, the build step works, and only the necessary files are included in the published package."

After publishing, verify it works:

> "Create a quick test project that installs our published package from npm and uses it. Verify it works as documented."

### Acceptance Criteria (Option B)

- [ ] **Identified and extracted** a reusable piece of code from your projects
- [ ] **Created a proper npm package** with TypeScript, build step, and correct package.json
- [ ] **Code is generalized** -- no app-specific dependencies or references
- [ ] **Comprehensive README** with description, installation, usage examples, and API reference
- [ ] **Comprehensive tests** with 20+ meaningful test cases, all passing
- [ ] **Published to npm** and verified it installs and works correctly
- [ ] **GitHub repository** with clean code and proper project structure

---

## Reflection (Both Options)

Write a brief reflection (in a blog post or markdown file) covering:

1. What you chose and why
2. What was the hardest part of understanding someone else's code (Option A) or generalizing your own code (Option B)?
3. What advanced Claude Code patterns did you use this week? Which were most effective?
4. How has your prompting skill evolved since Week 1?
