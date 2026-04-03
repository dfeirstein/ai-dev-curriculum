# Claude Code Prompting Patterns

> How to get the best results from Claude Code. Patterns that work, mistakes to avoid.

---

## The Core Workflow

Every task follows: **Research → Plan → Execute → Verify**

### 1. Research
Ask Claude Code to explore before building.

- "What are the best practices for [X]?"
- "How do most Next.js apps handle [X]?"
- "Compare [approach A] vs [approach B]. Which is better for our use case?"
- "Look at our codebase. How is [related feature] currently implemented?"

### 2. Plan
Ask Claude Code to design before coding.

- "Plan the architecture for [feature]. What files need to change? What's the data model?"
- "Enter plan mode. Design [X] considering [constraints]."
- "Before building, outline the approach. I want to review it first."

### 3. Execute
Direct Claude Code to build.

- "Build [X] following the plan we discussed."
- "Implement [feature] using [specific tools/libraries]."
- "Create a branch called feature/[name] and build [X]."

### 4. Verify
Check the output with quality gates.

- "Run lint, type-check, and build. Fix any issues."
- "Use the Chrome tool to test [user journey]."
- "Write tests for the critical paths we just built."

---

## Prompting Patterns

### Be Specific About What, Not How
Good: "Add a dark mode toggle to the header. Use shadcn/ui's theme system. Persist the choice in localStorage."
Bad: "Add dark mode."

### Include Constraints
Good: "Add a contact form. Use shadcn/ui form components, Zod for validation, and Resend to send the email. Fields: name, email, message. All required."
Bad: "Add a contact form."

### Reference Existing Patterns
Good: "Add a projects page. Follow the same layout pattern as the about page."
Bad: "Add a projects page."

### Ask for Options When Unsure
"Give me 3 approaches for handling [X]. Explain the tradeoffs of each."

### Break Down Big Features
Instead of: "Build the entire notification system."
Do it in steps:
1. "Plan the notification system architecture."
2. "Create the database schema for notifications."
3. "Build the API routes for creating and reading notifications."
4. "Build the notification bell UI component."
5. "Wire everything together."

### Iterate on Output
- "The spacing is too tight. Add more padding between sections."
- "This doesn't match what I asked for. I wanted [X], not [Y]."
- "Good, but also add [Z]."

---

## Quality Gate Prompts

Run these after every significant change:

- "Run lint and fix any issues." → `npm run lint`
- "Run type-check and fix any errors." → `npm run typecheck`
- "Run the build and fix any failures." → `npm run build`
- "Run all tests." → `npm run test`

Or create a skill: "Run /check" → does all of the above.

---

## Git Workflow Prompts

- "Create a branch called feature/[name]."
- "Commit with message: [describe the change]."
- "Push the branch and create a PR with a clear description."
- "Merge the PR and switch back to main."
- "What changed since the last commit?"
- "Revert the last commit." (when things go wrong)

---

## Project Setup Prompts

Starting a new project:
- "Create a Next.js project called [name] with TypeScript strict mode, Tailwind, shadcn/ui, ESLint, and Prettier."
- "Set up Drizzle ORM with a Neon PostgreSQL database. Here's the connection string: [X]."
- "Create a CLAUDE.md describing this project, the stack, and our coding conventions."

Adding to existing project:
- "Add Better Auth with email/password and GitHub OAuth."
- "Integrate Stripe with a Free and Pro tier. Use Checkout and webhooks."
- "Add Sentry for error monitoring and PostHog for analytics."

---

## Debugging Prompts

When something breaks:
- "I got this error: [paste error]. Fix it."
- "The [feature] isn't working. The expected behavior is [X] but it does [Y]."
- "Run the dev server and check the console for errors."

When Claude goes in circles:
- "Stop. Let's think about this differently. The actual problem is [X]."
- "Forget the previous approach. Start fresh with [new approach]."
- Start a new Claude Code session for fresh context.

---

## Review Prompts

- "Review the architecture of this project. What would you improve?"
- "Review the last 5 commits for any issues."
- "Are there any security concerns in the auth implementation?"
- "What's the most fragile part of this codebase?"

---

## Common Mistakes to Avoid

**Vague prompts:** "Make it better" → Better: "Improve the loading performance of the dashboard page."

**Too much at once:** "Build the entire app" → Better: Break it into features and build one at a time.

**Not verifying:** Always run quality gates. Claude Code can produce code that looks right but doesn't build.

**Ignoring the plan:** Skipping research and planning leads to rework. Spend time planning, save time building.

**Not using branches:** Build features on branches, not directly on main. PRs are your safety net.

**Accepting without reviewing:** Open the browser. Click around. Does it do what you asked? Quality gates help, but visual review matters too.
