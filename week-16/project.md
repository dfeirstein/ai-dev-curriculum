# Project: Polish and Ship Your Independent Project

**What you're building:** Taking your MVP from last week and making it production-ready.

**Time estimate:** 12-15 hours across the week

**What you'll need open:** Ghostty, your browser, Chrome DevTools

---

## The Brief

Your MVP works. This week, you make it professional. Testing, monitoring, CI/CD, performance, documentation. When you're done, this project should meet the same quality bar as TeamTask Pro.

No new features. Only quality, polish, and documentation.

---

## Phase 1: Testing (Days 1-2)

### Step 1: Add Vitest for Unit Tests

> "Add Vitest to this project with the proper configuration for Next.js and TypeScript. Create a test setup file."

### Step 2: Write Unit Tests for Core Logic

Identify the core business logic in your app. What are the functions, validations, or computations that matter most?

> "Write unit tests for [your core feature]. Test the happy path (valid inputs, expected behavior) and edge cases (empty inputs, invalid data, boundary conditions). Aim for at least 10 meaningful test cases."

Verify the tests pass:

> "Run the tests and show me the results."

### Step 3: Add E2E Tests

> "Set up end-to-end testing using Playwright with Chrome. Create E2E tests for the critical user flows: signing up, logging in, and [your core feature's main action]. Test both the happy path and common error scenarios."

Run the E2E tests and make sure they pass. Fix any failures before moving on.

### Step 4: Verify Test Coverage

> "Show me the test coverage report. Are there any critical paths that aren't covered?"

You don't need 100% coverage. You need the important paths covered: authentication, the core feature, and data persistence.

---

## Phase 2: Monitoring (Day 2-3)

### Step 5: Add Sentry Error Tracking

> "Set up Sentry error monitoring for this Next.js project. Configure it to capture both client-side and server-side errors. Add proper source maps for readable error traces."

Test that it works:

> "Add a temporary test button that throws an error so we can verify Sentry captures it. We'll remove it after testing."

Check your Sentry dashboard. See the error? Good. Remove the test button.

### Step 6: Add PostHog Analytics

> "Set up PostHog analytics for this project. Track page views automatically and add custom event tracking for key user actions: [list your core actions, e.g., 'creating a task', 'completing a booking', 'saving a recipe']."

Visit your app and perform some actions. Check your PostHog dashboard to verify events are being captured.

---

## Phase 3: CI/CD (Day 3)

### Step 7: Set Up GitHub Actions

> "Create a GitHub Actions workflow that runs on every push to main. The workflow should: install dependencies, run linting, run unit tests, and run E2E tests. Fail the build if any step fails."

Push a commit and watch the workflow run on GitHub. Everything should pass green.

### Step 8: Verify Automated Deployment

Confirm that your Vercel deployment triggers automatically on push to main. Push a small change and verify:

1. GitHub Actions runs and passes
2. Vercel detects the push and deploys
3. The live site reflects the change

This is your deployment pipeline. Code goes in, tests run, app deploys. No manual steps.

---

## Phase 4: Edge Cases and Polish (Day 3-4)

### Step 9: Handle Edge Cases

> "Review the entire app and add proper handling for: empty form submissions, very long text inputs, missing data, deleted resources (404 states), and network errors. Show user-friendly messages for all error conditions."

### Step 10: Add Loading and Empty States

> "Review every page and component that loads data. Add loading states (skeleton loaders or spinners) for when data is being fetched. Add empty states with helpful messages and calls-to-action for when there's no data to display."

### Step 11: Mobile Responsiveness

Open Chrome DevTools and toggle the device toolbar. Check your app at phone, tablet, and desktop widths.

> "Review the app for mobile responsiveness issues. Fix any layout problems, text overflow, or touch target sizes that are too small. Make sure the app is fully usable on mobile."

### Step 12: Visual Polish

Take a step back and look at your app with fresh eyes. Is it consistent? Do colors, spacing, and typography feel cohesive?

> "Do a visual consistency pass on the entire app. Ensure consistent spacing, typography, and color usage. Fix any visual inconsistencies or rough spots."

---

## Phase 5: Performance (Day 4)

### Step 13: Run Lighthouse Audit

Open Chrome DevTools, go to the Lighthouse tab, and run an audit on your main pages.

> "Here are my Lighthouse scores: Performance [X], Accessibility [X], Best Practices [X], SEO [X]. Help me improve any scores below 90. What are the biggest opportunities?"

Common fixes:
- Optimize and properly size images
- Add proper meta tags for SEO
- Ensure all interactive elements are accessible
- Reduce unnecessary JavaScript

### Step 14: Optimize Images and Assets

> "Optimize all images in the project. Use Next.js Image component with proper sizing and lazy loading. Convert any large images to WebP format."

---

## Phase 6: Documentation (Day 5)

### Step 15: Write the README

> "Write a comprehensive README.md for this project. Include:
> - Project name and a one-sentence description
> - A 'Features' section with bullet points
> - A 'Tech Stack' section listing all technologies used
> - A 'Getting Started' section with step-by-step setup instructions
> - A 'Environment Variables' section listing all required env vars with descriptions (no actual secrets)
> - A 'Testing' section explaining how to run tests
> - A 'Deployment' section explaining the CI/CD pipeline
> - A placeholder for screenshots (I'll add these manually)"

### Step 16: Take Screenshots

Take screenshots of your app's key screens:
- Home/landing page
- Main application view with data
- Core feature in action
- Mobile view

Save them in a `/screenshots` or `/docs` folder in your repo and update the README to reference them.

### Step 17: Final Deployment

Push everything to GitHub. Verify:

- [ ] GitHub Actions runs and passes
- [ ] Vercel deploys successfully
- [ ] The live site works correctly
- [ ] All features function as expected on the live site

---

## Acceptance Criteria

Your project is production-ready when it meets all of these:

- [ ] **Unit tests** covering core business logic (Vitest)
- [ ] **E2E tests** covering critical user flows (Playwright)
- [ ] **Error monitoring** with Sentry (errors captured and reportable)
- [ ] **Analytics** with PostHog (page views and key events tracked)
- [ ] **CI/CD pipeline** with GitHub Actions (tests run on every push)
- [ ] **Automated deployment** through Vercel (push to main = deploy)
- [ ] **Edge cases handled** (empty states, error states, loading states)
- [ ] **Mobile responsive** (works on phone, tablet, and desktop)
- [ ] **Lighthouse scores** of 90+ across all categories
- [ ] **README** with description, features, setup instructions, and screenshots
- [ ] **Live demo** at a working Vercel URL

---

## Reflection

You've now independently built a complete, production-ready application from scratch. Not a tutorial project. Not a guided exercise. Your idea, your decisions, your quality bar.

Look at what this project has:
- A real feature set that solves a real problem
- Authentication and data persistence
- Comprehensive testing
- Error monitoring and analytics
- Automated CI/CD
- Performance optimization
- Professional documentation

That's not a student project. That's a professional product. You built it by directing an AI. Take a moment to appreciate that.
