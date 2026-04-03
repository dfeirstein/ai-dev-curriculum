# Week 14 Resources

Reference material for CI/CD, GitHub Actions, performance optimization, and production readiness. Focus on understanding the concepts and configurations.

---

## GitHub Actions

- [GitHub Actions Documentation](https://docs.github.com/en/actions) -- The official docs. The "Quickstart" and "Understanding GitHub Actions" pages give a solid conceptual foundation.
- [GitHub Actions: Workflow Syntax](https://docs.github.com/en/actions/using-workflows/workflow-syntax-for-github-actions) -- Reference for the YAML workflow file. You won't write this yourself, but understanding the structure helps you review what Claude Code creates.
- [GitHub Actions: Caching Dependencies](https://docs.github.com/en/actions/using-workflows/caching-dependencies-to-speed-up-workflows) -- How caching works in CI. Caching node_modules between runs significantly speeds up your pipeline.
- [GitHub Actions: Secrets](https://docs.github.com/en/actions/security-for-github-actions/security-guides/using-secrets-in-github-actions) -- How to store and use sensitive values in workflows.

---

## Branch Protection

- [GitHub: Protected Branches](https://docs.github.com/en/repositories/configuring-branches-and-merges-in-your-repository/managing-protected-branches/about-protected-branches) -- Official documentation on branch protection rules. Covers all the options: required checks, required reviews, force push blocking.
- [GitHub: Managing Branch Protection Rules](https://docs.github.com/en/repositories/configuring-branches-and-merges-in-your-repository/managing-protected-branches/managing-a-branch-protection-rule) -- Step-by-step guide for configuring branch protection in the GitHub UI.

---

## Preview Deployments

- [Vercel: Preview Deployments](https://vercel.com/docs/deployments/preview-deployments) -- How Vercel creates unique deployments for every PR. Covers configuration, environment variables for previews, and custom domains for previews.
- [Vercel: Git Integration](https://vercel.com/docs/deployments/git) -- How Vercel connects to GitHub and responds to pushes and PRs.

---

## Performance

- [Lighthouse Documentation](https://developer.chrome.com/docs/lighthouse/overview) -- Official Lighthouse docs. The "Performance" section explains each metric and how to improve it.
- [Web.dev: Core Web Vitals](https://web.dev/articles/vitals) -- Google's definitive guide to Core Web Vitals (LCP, INP, CLS). Explains what each metric measures and why it matters.
- [Next.js: Image Optimization](https://nextjs.org/docs/app/building-your-application/optimizing/images) -- How the Next.js Image component works. Automatic sizing, format conversion, lazy loading, and responsive images.
- [Next.js: Caching](https://nextjs.org/docs/app/building-your-application/caching) -- Next.js caching mechanisms. Understanding the different cache layers (Request Memoization, Data Cache, Full Route Cache, Router Cache) conceptually helps you know what to ask Claude Code for.
- [Next.js: Lazy Loading](https://nextjs.org/docs/app/building-your-application/optimizing/lazy-loading) -- How to defer loading of heavy components. Covers next/dynamic and React.lazy().

---

## Environment Management

- [Vercel: Environment Variables](https://vercel.com/docs/projects/environment-variables) -- How to set different environment variables for Production, Preview, and Development environments.
- [The Twelve-Factor App: Config](https://12factor.net/config) -- The canonical reference for why configuration should be in the environment, not in code. Short and worth reading.

---

## CI/CD Philosophy

- [Martin Fowler: Continuous Integration](https://martinfowler.com/articles/continuousIntegration.html) -- The original article defining CI. Long but foundational. The first few sections explain the "why" clearly.
- [GitHub: CI/CD Explained](https://github.com/resources/articles/devops/ci-cd) -- A concise, modern explanation of CI/CD concepts. Good starting point before diving into GitHub Actions specifics.

---

## Secrets Management

- [GitHub: Security Best Practices for Actions](https://docs.github.com/en/actions/security-for-github-actions/security-guides/security-hardening-for-github-actions) -- How to keep secrets safe in CI/CD pipelines. Covers minimum permissions, secret scanning, and avoiding secret exposure in logs.
- [Vercel: Security](https://vercel.com/docs/security) -- How Vercel handles security for deployments, environment variables, and access control.

---

## A Note on These Resources

CI/CD and performance optimization have deep rabbit holes. You could spend weeks tuning Lighthouse scores or configuring elaborate multi-stage deployment pipelines. Don't. The goal this week is a solid foundation: automated CI, preview deployments, branch protection, and scores above 90. That's production-ready. You can optimize further when you have real users and real performance data to guide your decisions.
