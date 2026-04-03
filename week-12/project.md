# Project: TeamTask Pro — Testing and Quality

**What you're building:** A comprehensive test suite for TeamTask Pro -- unit tests, integration tests, and end-to-end tests that verify the application works correctly.

**Time estimate:** 8-10 hours

**What you'll need open:** Ghostty (your terminal) and a web browser

---

## The Brief

TeamTask Pro is a live product. Changes you make from now on could break existing features. This week you're adding the safety net: automated tests that catch bugs before users do.

You'll set up Vitest for unit and integration tests, write tests for every critical path, and use the Claude Chrome tool to test complete user journeys. When you're done, running a single command will verify that everything in TeamTask Pro works correctly.

---

## Phase 1: Research

Start Claude Code and research the testing approach:

> "Claude, I have a Next.js app (TeamTask Pro) with TypeScript, Drizzle ORM, Better Auth, Stripe integration, and various API routes for tasks, projects, members, and organizations. I want to add comprehensive testing. What's the best way to set up Vitest for this project? How should I handle testing API routes that need auth and database access?"

Follow up:

> "How do I set up a test database so tests don't affect production data?"

> "What's the best approach for testing authenticated API routes -- how do I simulate a logged-in user in tests?"

> "Which parts of TeamTask Pro should I prioritize for testing?"

---

## Phase 2: Plan

> "Claude, plan a comprehensive test suite for TeamTask Pro. I want: (1) Vitest set up and configured for this project, (2) unit tests for all utility functions, Zod validation schemas, and helper functions, (3) integration tests for all API routes covering auth, CRUD, Stripe webhooks, and authorization, (4) E2E tests using the Chrome tool for critical user journeys, (5) a /test skill that runs the full suite. Prioritize the tests by criticality -- what's most important to test first?"

Review the plan. Key questions:

- What test infrastructure is needed (test database, auth mocking, etc.)?
- What are the critical paths that absolutely need tests?
- How will tests be organized in the project?
- What's the expected run time for the full suite?

---

## Phase 3: Execute

### Step 1: Set Up Vitest

> "Claude, set up Vitest for this project. Configure it to work with TypeScript, Next.js path aliases, and our existing project structure. Set up a test database (a separate Postgres database or an in-memory alternative) so tests don't affect development data. Add test scripts to package.json: 'test' for running all tests, 'test:unit' for unit tests only, 'test:integration' for integration tests only. Create a test setup file that handles database seeding and cleanup."

Verify the setup:

> "Claude, create a simple smoke test -- just a test that asserts true equals true -- and run it with npm run test to make sure Vitest is working."

### Step 2: Unit Tests for Validation Schemas

> "Claude, write unit tests for all Zod validation schemas in TeamTask Pro. For each schema, test: (1) valid data passes validation, (2) required fields are enforced -- missing each required field should fail, (3) invalid types are rejected -- string where number expected, etc., (4) edge cases -- empty strings, very long strings, special characters, negative numbers where positive expected. Cover the schemas for: task creation/update, project creation/update, member invitation, and organization creation."

Run and verify:

> "Claude, run npm run test:unit and show me the results. Fix any failing tests."

### Step 3: Unit Tests for Utility Functions

> "Claude, write unit tests for all utility functions in TeamTask Pro. This includes: date formatting functions, CSV generation, role checking helpers, permission utilities, activity log formatters, and any data transformation functions. Test normal cases, edge cases, and error cases for each."

Run and verify:

> "Claude, run npm run test:unit and show me the results."

### Step 4: Integration Tests for Auth Routes

> "Claude, write integration tests for the authentication API routes. Test: (1) signing up with valid credentials creates a user and returns a session, (2) signing up with an existing email returns an error, (3) signing up with an invalid email format returns a validation error, (4) logging in with correct credentials returns a session, (5) logging in with wrong password returns an error, (6) accessing a protected route without auth returns 401, (7) logging out invalidates the session."

Run and verify:

> "Claude, run npm run test:integration and show me the results. Fix any failing tests."

### Step 5: Integration Tests for Core CRUD

> "Claude, write integration tests for the task and project API routes. For tasks: test creating a task, reading tasks (only returns org's tasks), updating a task, completing a task (verify activity log entry is created), deleting a task, and creating a task with invalid data. For projects: test creating a project, reading projects, updating a project, archiving a project, and creating a project with invalid data. All tests should authenticate as a valid user and verify org-scoping -- a user should never see another org's data."

Run and verify:

> "Claude, run npm run test:integration and show me the results."

### Step 6: Integration Tests for Stripe Webhooks

> "Claude, write integration tests for the Stripe webhook handlers. Test: (1) a checkout.session.completed webhook updates the subscription status, (2) a customer.subscription.updated webhook updates the plan details, (3) a customer.subscription.deleted webhook marks the subscription as canceled, (4) a webhook with an invalid signature is rejected, (5) a webhook for an unknown event type is handled gracefully. Mock the Stripe webhook signature verification for testing."

Run and verify:

> "Claude, run npm run test:integration and show me the results."

### Step 7: E2E Tests with Chrome Tool

> "Claude, use the Chrome tool to test the complete signup-to-task-completion journey on the local dev server. Here's the flow: (1) Navigate to the app's homepage, (2) Click 'Sign Up', (3) Fill in a test email and password, (4) Submit the signup form, (5) Verify the dashboard loads, (6) Create a new project called 'E2E Test Project', (7) Create a task called 'E2E Test Task' in that project, (8) Mark the task as complete, (9) Navigate to the dashboard and verify the completed task count increased, (10) Navigate to the activity log and verify entries for the project creation, task creation, and task completion."

> "Claude, use the Chrome tool to test the admin access control. Log in as a regular member (non-admin), navigate to /admin, and verify it redirects away or shows an access denied message. Then log in as an admin and verify the admin panel loads correctly."

> "Claude, use the Chrome tool to test the CSV export. Log in, navigate to the export page or dashboard, click the 'Export Tasks' button, and verify a file downloads."

### Step 8: Create the /test Skill

> "Claude, create a /test skill in the CLAUDE.md file that runs the full quality pipeline in order: (1) npm run lint, (2) npm run typecheck, (3) npm run test, (4) npm run build. If any step fails, stop and report the failure. Show a summary at the end with pass/fail status for each step and the total test count."

Test the skill:

> "/test"

Verify it runs all four gates in order and reports results clearly.

### Step 9: Fix and Iterate

After running the full suite, there will likely be failures. This is normal and expected.

> "Claude, the integration tests for task creation are failing with [error]. The test expects X but gets Y. Fix the test if the behavior is correct, or fix the code if the behavior is wrong."

The key judgment: when a test fails, is the test wrong or is the code wrong? If the code is doing the right thing and the test expectation is off, fix the test. If the code has a bug the test exposed, fix the code. This is the value of testing.

### Step 10: Quality Gates and Deploy

> "Claude, run the full quality pipeline -- lint, typecheck, test, build. All gates must pass."

> "Claude, commit all changes with a clear message about the test suite, push to GitHub, and deploy."

---

## Acceptance Criteria

Your project is complete when all of these are true:

- [ ] Vitest is set up and configured for the project
- [ ] Unit tests exist for all Zod validation schemas
- [ ] Unit tests exist for utility functions (date formatting, CSV generation, role checks, etc.)
- [ ] Integration tests exist for authentication routes (signup, login, logout, protected routes)
- [ ] Integration tests exist for task CRUD routes (create, read, update, delete, complete)
- [ ] Integration tests exist for project CRUD routes
- [ ] Integration tests exist for Stripe webhook handlers
- [ ] E2E test of the complete signup-to-task-completion journey passes via Chrome tool
- [ ] E2E test of admin access control passes via Chrome tool
- [ ] A /test skill runs the full quality pipeline (lint, typecheck, test, build)
- [ ] All tests pass
- [ ] All quality gates pass (lint, typecheck, test, build)
- [ ] Code is on GitHub
- [ ] App is deployed and functional on Vercel

---

## Stretch Goals

**Test Coverage Report**
> "Claude, configure Vitest to generate a test coverage report. Show coverage percentages for each file and highlight any critical files below 80% coverage. Add a coverage threshold to fail the test run if critical paths drop below 80%."

**Snapshot Tests for API Responses**
> "Claude, add snapshot tests for the API response shapes. When the shape of an API response changes unexpectedly, the test should fail. This catches accidental breaking changes to the API contract."

**Load Testing**
> "Claude, write a simple load test that sends 100 concurrent requests to the task listing API. Verify that all requests return correct data and none fail. This tests whether the app handles concurrent usage."

**Error Scenario E2E Tests**
> "Claude, use the Chrome tool to test error scenarios: try to sign up with an email that already exists, try to create a task without a title, try to access another org's project via URL manipulation. Verify the app handles each gracefully with appropriate error messages."

---

## Troubleshooting

**Vitest won't run**
> "Claude, Vitest fails to start with this error: [paste error]. Check the Vitest configuration, TypeScript setup, and path alias configuration."

**Tests can't connect to the database**
> "Claude, integration tests fail with a database connection error. Check the test database configuration -- is the test database URL set in the test environment? Is the test database created and migrated?"

**Auth tests fail with session errors**
> "Claude, the auth integration tests fail because sessions aren't being created properly in the test environment. Check how we're simulating authentication in tests -- do we need to mock the auth middleware or create real sessions?"

**Chrome tool can't find elements on the page**
> "Claude, the E2E test failed because it couldn't find the signup button. The Chrome tool reports the element isn't visible. Check if the dev server is running, the page is loading correctly, and the element selectors match the actual page."

**Tests pass locally but build fails**
> "Claude, all tests pass when I run npm run test, but npm run build fails. Check for TypeScript errors in test files, import issues, or configuration problems that only surface during the build step."

---

## Reflection

You now have a safety net. Every time you make a change to TeamTask Pro, you can run the test suite and know within seconds whether you've broken something. That's a fundamentally different experience than manually clicking through the app and hoping for the best.

Notice the workflow: you described the behavior you wanted to verify, Claude Code wrote the tests, and you ran them to confirm they pass. You didn't write test code. You thought about what matters, what could go wrong, and what "working correctly" means. That's the skill -- the AI handles the implementation.
