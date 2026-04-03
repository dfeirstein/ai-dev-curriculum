# Week 16 Guide: Polish, Quality, and Shipping

---

## Part 1: The Last 20%

### Why Polish Takes So Long

You built your MVP in about five days. Core features work. Users can do the main thing. So why does it take another full week to get it production-ready?

Because software in the wild is unpredictable. Users do things you didn't expect. Networks fail. Browsers behave differently. Screens come in sizes you didn't test. Data arrives in formats you didn't plan for. Every one of these scenarios needs to be handled gracefully -- or at least not crash.

The MVP phase is about answering: "Can this work?" The polish phase is about answering: "Does this work reliably, in all conditions, for all users?"

### The Edge Cases

Edge cases are the situations that aren't the main path. They're what happens when:

- A user submits an empty form
- A user enters a 10,000-character string where you expected 50
- The database is temporarily unreachable
- A user clicks a button twice rapidly
- A user visits a page for an item that was deleted
- A user has no data yet (the empty state)
- A user has a massive amount of data (the overflow state)

You don't need to handle every conceivable edge case. You need to handle the common ones gracefully. "Gracefully" means the app doesn't crash, the user sees a helpful message, and no data is corrupted.

Direct Claude Code to think about these:

> "Review the [feature name] and add error handling for edge cases: empty inputs, invalid data, network errors, and empty states. Show a user-friendly error message instead of crashing."

### Error States vs. Loading States vs. Empty States

Three states that every page and feature needs to handle:

**Loading states:** What does the user see while data is being fetched? A blank page is bad. A spinner or skeleton screen tells the user something is happening.

**Error states:** What does the user see when something goes wrong? A cryptic error message is bad. A clear message with a suggested action ("Try again" or "Go back to dashboard") is good.

**Empty states:** What does the user see when there's no data yet? A blank page with just headers feels broken. A message like "No tasks yet. Create your first task to get started." with a clear call-to-action feels intentional and helpful.

> "Claude, review every page in the app and add proper loading states, error states, and empty states. Use skeleton loaders for loading, friendly error messages with retry options for errors, and helpful prompts with calls-to-action for empty states."

---

## Part 2: The Quality Checklist

You've done all of this before with TeamTask Pro. Now you're applying the same playbook to your own project, independently. Here's the checklist:

### Testing

- **Unit tests with Vitest:** Test your core business logic. Data validation, utility functions, key computations.
- **E2E tests with Chrome:** Test the critical user flows. Can a user sign up, perform the core action, and see the result?
- **Test the happy path and the sad path.** What happens when everything goes right? What happens when something goes wrong?

### Monitoring

- **Sentry for error tracking:** Catch runtime errors before your users report them.
- **PostHog for analytics:** Understand how people actually use your app. Which features get used? Where do people drop off?

### CI/CD

- **GitHub Actions:** Run tests automatically on every push. Catch regressions before they reach production.
- **Automated deployment:** Push to main, tests run, code deploys. No manual steps.

### Performance

- **Lighthouse audit:** Run Lighthouse in Chrome DevTools. Target 90+ on Performance, Accessibility, Best Practices, and SEO.
- **Image optimization:** Are images properly sized and in modern formats?
- **Bundle size:** Is the JavaScript payload reasonable?
- **Core Web Vitals:** Does the page load fast? Is it interactive quickly? Does the layout stay stable?

You don't need to memorize how to set any of this up. You direct Claude Code:

> "Add Vitest to this project and write unit tests for the core business logic in [feature]."

> "Set up Sentry error monitoring for this Next.js project."

> "Create a GitHub Actions workflow that runs tests on every push to main."

You've given these exact prompts before. The difference this week is that you're deciding what to test and monitor, not following instructions.

---

## Part 3: Documentation

### Why Documentation Matters

Your README is the first thing someone sees when they visit your GitHub repository. It's also the first thing a hiring manager, collaborator, or potential user reads. A great README can make a mediocre project look professional. A missing README makes even an excellent project look abandoned.

### What Every Project README Needs

1. **Project name and one-line description.** What is this? In one sentence.
2. **Screenshot or demo GIF.** Show, don't tell. A screenshot is worth a thousand words in a README.
3. **Live demo link.** If it's deployed, link to it. Let people try it immediately.
4. **Features list.** Bullet points of what the app does. Keep it concise.
5. **Tech stack.** What tools and technologies are used. This tells other developers what they're looking at.
6. **How to run locally.** Clone, install, set up env vars, run. Step by step.
7. **Environment variables.** What's needed, what each one is for (without revealing secrets).

### Writing the README with Claude Code

> "Write a comprehensive README.md for this project. Include: a project description, screenshot placeholder, live demo link at [your URL], a features list, the tech stack, instructions for running locally, and a list of required environment variables with descriptions."

Then replace the screenshot placeholder with an actual screenshot. Take one in your browser, save it, and add it to the repo.

---

## Part 4: Demo Preparation

### Why Demos Matter

You'll demo this project in your portfolio (Week 19). But even now, practicing how you present your work is valuable. Every project you ship should have a clear narrative:

1. **The problem.** What issue does this solve? Why does it matter?
2. **The solution.** What did you build? Show the core feature.
3. **The craft.** What makes it production-quality? Testing, monitoring, performance.
4. **The journey.** What decisions did you make? What was challenging?

### Screenshots

Take screenshots of:
- The landing page or home screen
- The core feature in action
- The dashboard or main view with data
- Any particularly polished UI element

Save these in your repo and reference them in your README.

---

## Part 5: The Shipping Mindset

### "Done" Is Better Than "Perfect"

There will always be one more thing to add, one more edge case to handle, one more visual tweak to make. At some point, you have to decide it's done.

Here's the test: Does it work? Is it tested? Is it monitored? Can someone understand it from the README? If yes, it's done. Ship it. Move on. You can always come back later.

The ability to ship -- to actually finish something and put it in the world -- is one of the most valuable skills in software development. Many developers start dozens of projects and finish none. You're going to finish this one.

### The Professional Standard

What you're building this week isn't just a school project. It's a portfolio piece. It's something you'll show to potential employers, clients, or collaborators. Treat it accordingly:

- Clean up any placeholder text or lorem ipsum
- Make sure all links work
- Verify the deployed version matches what you expect
- Test on mobile (or at least narrow your browser window)
- Read through the README as if you've never seen the project before

When you're done, you should feel genuinely proud of what you've built. Not because it's perfect -- because it's real, it works, and you shipped it.
