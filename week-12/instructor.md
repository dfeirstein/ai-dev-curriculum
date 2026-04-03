# Week 12: TeamTask Pro — Testing — Instructor Guide

## Week at a Glance
- Theme: Automated testing with Vitest for unit/integration tests and Claude Chrome tool for end-to-end tests. Students learn that testing protects the product they just launched.
- What students ship: A test suite with unit tests, integration tests, and at least 3 E2E tests covering critical user flows. Tests must pass in CI.
- Estimated hours: 12-16
- Difficulty: 4

## Pacing Guide
- **Day 1-2:** Students read guide.md and understand the testing pyramid. Direct Claude Code to set up Vitest and write unit tests for utility functions (date formatting, validation schemas, role checks, CSV generation). Then move to integration tests for 2-3 API routes.
- **Day 3-4:** Integration tests for critical API routes (task CRUD, auth flows, Stripe webhooks). Then introduce the Claude Chrome tool for E2E testing. Students direct Claude Code to write E2E tests for signup, task creation, and dashboard rendering.
- **Day 5:** Fix flaky tests, ensure all tests pass consistently, add test scripts to package.json, verify tests run in CI. Ship the tested codebase.

## Getting Students Started
- The biggest barrier this week is motivation. Students just shipped V1 and are riding a high. Testing feels like a step backward. Address this directly: "You shipped something real. Testing is how you keep it working as you keep building."
- Common confusion: students do not know what to test first. Give them a concrete starting point -- "Ask Claude Code to set up Vitest and write a test for your Zod task validation schema."
- Students should have open: Claude Code CLI, the repo, and a browser with the deployed app (for E2E reference). They should also have realistic seed data in their dev database.

## Check-in Points
- **After guide.md:** Verify the student can explain the difference between unit, integration, and E2E tests in their own words. They should be able to name 2-3 specific things they would test at each level in TeamTask Pro.
- **Mid-project checkpoint:** By end of Day 3, the student should have passing unit tests and at least 2 integration tests. If they are behind, have them skip integration tests and focus on unit tests + 1 E2E test. A small passing suite is better than a large broken one.
- **Pre-ship review:** All tests pass when running `npm test`. No skipped tests. E2E tests run without manual intervention. The student can run the full suite and show green output.

## Common Pitfalls
- **"Testing is pointless, my app already works."** This is the most common resistance. Counter with a concrete scenario: "Change your task validation schema slightly and see what breaks. Now imagine you made that change and didn't notice." Let them experience a regression firsthand.
- **E2E tests are flaky.** The Claude Chrome tool can produce tests that pass sometimes and fail others due to timing issues. Teach students to direct Claude Code to add explicit waits, retry logic, and to test for element visibility before interaction. If a test is consistently flaky after 3 attempts, it is acceptable to mark it as a known issue and move on.
- **Testing implementation details instead of behavior.** Students sometimes write tests that check internal state rather than observable output. Guide them: "Test what the user sees or what the API returns, not how the code internally processes it."
- **Mocking everything.** Students may direct Claude Code to mock the database for integration tests, defeating the purpose. Integration tests should hit a real test database. Unit tests can mock external dependencies.
- **Spending all week on test setup.** Vitest config, TypeScript path aliases, and test database setup can eat an entire day. If a student is stuck on config for more than 2 hours, intervene directly. Have them share the error and help them direct Claude Code to resolve it.

## How to Know the Student Gets It
- The student can explain what each test is checking and why it matters.
- The student writes tests for edge cases, not just the happy path (e.g., "what happens when a non-member tries to access a task?").
- The student treats a failing test as useful information rather than an annoyance.
- When a test fails, the student investigates whether the test is wrong or the code is wrong before just deleting the test.
- **Red flags:** Student writes 30 trivial tests (testing that `1 + 1 === 2` equivalents) to hit a number. Student deletes failing tests instead of fixing them. Student cannot run the test suite without your help.

## Support Strategies
- Pair with the student for the first unit test. Walk through the prompt they give Claude Code and review the output together. Once they see the pattern, they can repeat it independently.
- If E2E tests are causing too much frustration, have the student write a manual test checklist instead and focus energy on unit and integration tests. E2E coverage is valuable but not worth a student shutting down.
- For database-related test failures (connection issues, missing tables), check that the student has a separate test database configured and that migrations have run against it.

## Differentiation
- **Moving fast?** Add test coverage reporting with `vitest --coverage`. Challenge them to reach 60% coverage on critical modules. Write E2E tests for the full payment flow. Add snapshot tests for key UI components.
- **Struggling?** Reduce scope to 5 unit tests for utility functions and 1 E2E test for the login flow. The goal is that they understand the concept and have a working test command, not that they have comprehensive coverage.
- **Minimum viable ship:** Vitest installed, 3 passing unit tests, `npm test` runs without errors.
