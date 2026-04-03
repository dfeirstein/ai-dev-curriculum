# Week 12 Resources

Reference material for testing concepts, Vitest, and end-to-end testing. Focus on understanding the philosophy of testing -- not test syntax.

---

## Testing Philosophy

- [Testing Trophy (Kent C. Dodds)](https://kentcdodds.com/blog/the-testing-trophy-and-testing-classifications) -- An alternative to the testing pyramid that emphasizes integration tests. Good conceptual framework for thinking about where to invest testing effort.
- [Write Tests. Not Too Many. Mostly Integration.](https://kentcdodds.com/blog/write-tests) -- A concise argument for pragmatic testing. Matches the AI-native approach: test what matters, don't chase coverage numbers.

---

## Vitest

- [Vitest Documentation](https://vitest.dev/) -- The official docs. The "Getting Started" and "Features" sections are worth skimming to understand what Vitest provides.
- [Vitest with Next.js](https://nextjs.org/docs/app/building-your-application/testing/vitest) -- Next.js official guide to setting up Vitest. Claude Code follows this pattern when configuring your project.

---

## Integration Testing

- [Testing API Routes in Next.js](https://nextjs.org/docs/app/building-your-application/testing) -- Official Next.js testing documentation. Conceptual overview of how to test server-side code.
- [MSW (Mock Service Worker)](https://mswjs.io/) -- A tool for mocking API responses in tests. Useful conceptual reference for understanding how integration tests handle external services like Stripe.

---

## End-to-End Testing

- [Claude Code Chrome Tool](https://docs.anthropic.com/en/docs/claude-code/tutorials/computer-use-web-testing) -- Documentation for using Claude's browser control capabilities for testing. This is the primary E2E testing approach in this curriculum.
- [Playwright Documentation](https://playwright.dev/docs/intro) -- A traditional E2E testing framework. Worth understanding conceptually even though you'll primarily use the Chrome tool. The "Why Playwright" section explains the value of browser-based testing.

---

## Testing Patterns

- [Arrange-Act-Assert](https://automationpanda.com/2020/07/07/arrange-act-assert-a-pattern-for-writing-good-tests/) -- The fundamental pattern for structuring any test: set up the conditions, perform the action, verify the result. Understanding this pattern helps you describe tests to Claude Code more clearly.
- [Testing Best Practices (Goldbergyoni)](https://github.com/goldbergyoni/javascript-testing-best-practices) -- A comprehensive list of JavaScript testing best practices. Skim the section headers to understand the landscape. The "Golden Rule" and "Test Anatomy" sections are most relevant.

---

## Test Coverage

- [Istanbul / c8 Coverage](https://vitest.dev/guide/coverage) -- Vitest's built-in coverage tooling. Conceptual understanding: coverage tells you which lines of code were executed during tests, helping you find untested paths.

---

## Quality Pipelines

- [What Is CI/CD? (GitHub)](https://github.com/resources/articles/devops/ci-cd) -- A conceptual overview of continuous integration and continuous deployment. You'll automate the pipeline in Week 14, but understanding the concept now helps you appreciate why the lint-typecheck-test-build sequence matters.

---

## A Note on These Resources

You'll notice a theme in the testing resources: they focus more on what to test and why than on how to write test code. That's intentional. Your job is to decide which behaviors matter, which edge cases could cause problems, and which user journeys are critical. Claude Code writes the actual test code. The thinking is yours; the typing is the AI's.
