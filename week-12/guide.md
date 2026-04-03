# Week 12 Guide: Testing and Quality

You shipped v1. Now imagine this: a user signs up, creates an organization, invites their team, sets up a project, and starts adding tasks. Everything works. Then you push an update -- maybe a small fix to the dashboard -- and suddenly the signup flow is broken. Nobody can create new accounts. You don't know about it until a frustrated user emails you three days later.

This is what testing prevents. Automated tests verify that your application works correctly, and they re-verify every time you make a change. Tests are not about proving your code is perfect. They're about catching when something that used to work suddenly doesn't.

---

## Part 1: Why Testing Matters

### The Real Reason

Testing isn't a best practice you adopt because someone told you to. Testing exists because software is interconnected. Changing one thing can break another thing that seems completely unrelated.

In TeamTask Pro, the task creation API route uses the auth system to identify the user, the database schema to validate the data, the activity log to record the event, and the notification system to alert assignees. A change to any of those systems could break task creation. Without tests, the only way to know is to manually try every feature after every change. That doesn't scale.

### What Tests Give You

**Confidence to change things.** When your tests pass, you know the critical paths still work. You can refactor, add features, and fix bugs without fear that you've broken something else.

**Documentation of behavior.** A test that says "when a user creates a task without a title, it should return a validation error" is documentation. It tells anyone (including future you) exactly what the application is supposed to do.

**Faster feedback.** Running a test suite takes seconds. Manually clicking through every feature takes hours. Tests catch problems in seconds that manual testing might miss entirely.

### What Tests Don't Give You

Tests don't guarantee your app is bug-free. They guarantee that the specific scenarios you've tested work correctly. The art of testing is choosing which scenarios matter most.

---

## Part 2: Types of Tests

### Unit Tests (Vitest)

Unit tests test individual functions in isolation. One function, one test. Does this specific piece of logic work correctly?

**Example scenarios for TeamTask Pro:**
- Does the date formatting function produce the right output?
- Does the Zod validation schema reject a task with an empty title?
- Does the role-check function correctly identify admins?
- Does the CSV generation function produce valid CSV output?

Unit tests are fast (milliseconds each), easy to write, and easy to understand. They test the building blocks of your application.

**Vitest** is the test runner you'll use. It's fast, works natively with TypeScript, and integrates well with Next.js. You don't need to understand Vitest's internals. You just need to know it exists and what it does -- Claude Code handles the rest.

### Integration Tests

Integration tests test multiple pieces working together. Instead of testing a function in isolation, you test an API route end-to-end: does a request come in, get processed correctly, hit the database, and return the right response?

**Example scenarios for TeamTask Pro:**
- Send a POST request to create a task -- does it return the created task with the correct data?
- Send a GET request for tasks without authentication -- does it return a 401 error?
- Send a request to complete a task -- does the task status change AND does an activity log entry get created?
- Send a webhook event from Stripe -- does the subscription status update correctly?

Integration tests are slower than unit tests (they hit the database, make HTTP requests) but they test real-world scenarios more realistically.

### End-to-End Tests (Claude Chrome Tool)

End-to-end (E2E) tests test the entire application from a user's perspective. They open a real browser, navigate to pages, click buttons, fill out forms, and verify the results. They test everything: frontend, backend, database, auth, the works.

**Example scenarios for TeamTask Pro:**
- Navigate to the signup page, create an account, verify the dashboard loads
- Log in, create a project, add a task, mark it complete, verify the dashboard chart updates
- Try to access the admin panel as a non-admin, verify it redirects

E2E tests are the slowest and most brittle (they depend on the UI not changing), but they're the most realistic. They test what actual users experience.

The **Claude Chrome tool** is uniquely powerful here. Instead of writing test scripts in a specialized testing framework, you describe the user journey in plain English:

> "Claude, use the Chrome tool to navigate to the signup page, create an account with a test email, verify the dashboard loads, then create a project called 'Test Project' and verify it appears in the project list."

Claude literally opens a browser, performs the actions, and verifies the results. This is testing that matches how you think about your application -- in terms of user journeys, not code paths.

---

## Part 3: The Testing Pyramid

### The Shape of a Good Test Suite

The testing pyramid is a simple mental model:

```
       /\
      / E2E\        Few -- slow, expensive, but realistic
     /______\
    /Integration\   Some -- moderate speed, test real interactions
   /______________\
  /   Unit Tests    \  Many -- fast, cheap, test the building blocks
 /____________________\
```

**Many unit tests.** They're fast and catch logic bugs. Write lots of them for every utility function, validation schema, and data transformation.

**Some integration tests.** They're slower but catch interaction bugs. Write them for every API route and critical data flow.

**Few E2E tests.** They're slow and can be flaky, but they catch the bugs that matter most -- the ones users actually experience. Write them for the critical user journeys: signup, login, core task management flow.

### Why This Shape?

It's about economics. Unit tests give you the most confidence per second of execution time. E2E tests give you the most realistic coverage but take the longest to run. You want a broad base of fast tests and a small cap of slow but realistic tests.

---

## Part 4: What to Test vs. What Not to Test

### Test These Things

**Critical user paths.** The flows that, if broken, mean your app is unusable:
- Authentication (signup, login, logout)
- Core CRUD operations (create task, update task, complete task, delete task)
- Payment flows (subscription creation, webhook handling)
- Authorization (admin-only routes, org-scoped data)

**Validation logic.** Every Zod schema, every input validation function. These are your first line of defense against bad data.

**Edge cases.** What happens with empty strings? Very long strings? Special characters? Missing fields? Duplicate entries? These are where bugs hide.

**Business logic.** Feature gating (free vs. pro limits), role-based access, task assignment rules.

### Don't Test These Things

**Third-party libraries.** Don't test that Stripe's SDK works. Stripe tests that. Test that your integration with Stripe works correctly.

**UI styling.** Don't test that a button is blue. Test that clicking the button does the right thing.

**Framework behavior.** Don't test that Next.js routes requests correctly. Test that your route handlers process requests correctly.

**Trivial code.** Don't test a function that just returns a constant. Test functions that make decisions.

### The 80/20 Rule

You don't need 100% test coverage. You need coverage on the 20% of code that causes 80% of bugs. That's almost always: input validation, authentication, authorization, payment handling, and data mutations (creates, updates, deletes).

---

## Part 5: Telling Claude Code to Write Tests

### Describe Behavior, Not Code

When directing Claude Code to write tests, describe what the application should DO, not how the test code should look.

**Good direction:**
> "Write tests for the task creation API route. Test that: a logged-in user can create a task with a valid title, creating a task without a title returns a validation error, creating a task without authentication returns 401, and the created task belongs to the correct organization."

**Not helpful:**
> "Write a test file with describe blocks and it blocks for the task API."

The first tells Claude Code what behavior to verify. The second tells it what syntax to use. You care about the behavior.

### Testing Zod Schemas

Zod schemas are perfect candidates for unit tests because they have clear input-output behavior:

> "Write unit tests for all the Zod validation schemas in the project. For each schema, test that valid data passes, required fields are enforced, and invalid data types are rejected. Pay special attention to edge cases: empty strings, very long strings, and missing optional fields."

### Testing API Routes

API routes need integration tests that make real HTTP requests:

> "Write integration tests for the task CRUD API routes. Set up a test database. Test creating, reading, updating, and deleting tasks. Test authentication -- unauthenticated requests should return 401. Test authorization -- users should only see tasks from their own organization."

### Testing with the Chrome Tool

For E2E tests, describe the user journey:

> "Claude, use the Chrome tool to test the complete signup flow: navigate to the app, click 'Sign Up', fill in an email and password, submit the form, and verify the dashboard loads with a welcome message."

---

## Part 6: The Quality Pipeline

### What a Pipeline Is

A quality pipeline is a sequence of automated checks that your code must pass before it's considered ready. Each check is a gate. If any gate fails, the pipeline fails.

For TeamTask Pro, the pipeline is:

1. **Lint** (`npm run lint`) -- Check for code style issues and common mistakes.
2. **Type-check** (`npm run typecheck`) -- Check that all types are correct. TypeScript catches structural bugs.
3. **Test** (`npm run test`) -- Run the test suite. Verify behavior is correct.
4. **Build** (`npm run build`) -- Compile the application. Verify everything compiles.

All four gates must pass. If lint catches an issue, fix it before running tests. If tests fail, fix them before building. The pipeline ensures that every change meets a quality bar.

### Running the Pipeline

You'll eventually automate this with CI/CD (Week 14). For now, you run it manually:

> "Claude, run the full quality pipeline: lint, typecheck, test, then build. Fix any issues that come up at each stage."

### The /test Skill

You'll create a Claude Code skill that runs the full test suite with a single command. Instead of remembering which commands to run, you just type `/test` and everything executes in order.

---

## Part 7: How the Chrome Tool Works for Testing

### What It Is

The Claude Chrome tool lets Claude Code control a real web browser. It can navigate to URLs, click buttons, fill out forms, read page content, and take screenshots. For testing, this means Claude can perform the same actions a user would and verify the results.

### What Makes It Special

Traditional E2E testing tools (Playwright, Cypress) require writing test scripts in a specific syntax. The Chrome tool lets you describe tests in plain English. Claude interprets your description, performs the actions, and reports what happened.

This matches the AI-native philosophy: you describe WHAT to verify, Claude figures out HOW to verify it.

### Example Flow

You say:
> "Claude, use the Chrome tool to navigate to the signup page, create an account, and verify the dashboard loads."

Claude:
1. Opens Chrome and navigates to your app's URL
2. Finds and clicks the "Sign Up" link
3. Fills in the email and password fields
4. Clicks the submit button
5. Waits for the page to load
6. Checks that the dashboard page appeared with the expected elements
7. Reports success or failure, with a screenshot if something went wrong

### When to Use It

Use the Chrome tool for testing flows that span multiple pages and involve real browser interactions: signup, login, creating entities, navigating between pages, and verifying visual outcomes. These are the tests that are hardest to write as code but easiest to describe in English.

---

## What's Next

Head to [project.md](project.md) to add automated testing to TeamTask Pro.
