# Week 13 Resources

Reference material for error monitoring, product analytics, feature flags, and privacy. Focus on understanding the concepts and what each tool provides.

---

## Sentry

- [Sentry for Next.js](https://docs.sentry.io/platforms/javascript/guides/nextjs/) -- The official integration guide. The "Getting Started" section covers the setup Claude Code will implement.
- [Sentry Source Maps](https://docs.sentry.io/platforms/javascript/sourcemaps/) -- How source maps work and why they matter for production debugging. Conceptual understanding of the mapping between compiled and original code.
- [Sentry Alerts](https://docs.sentry.io/product/alerts/) -- How to configure alert rules. Browse this when setting up alerts in the Sentry dashboard.
- [Sentry Releases](https://docs.sentry.io/product/releases/) -- How release tracking works -- correlating errors with specific deploys.
- [Sentry Error Boundaries (React)](https://docs.sentry.io/platforms/javascript/guides/react/features/error-boundary/) -- How React error boundaries integrate with Sentry to catch and report rendering errors.

---

## PostHog

- [PostHog Documentation](https://posthog.com/docs) -- The getting started guide. Browse the product analytics section for the conceptual overview.
- [PostHog for Next.js](https://posthog.com/docs/libraries/next-js) -- Specific integration guide for Next.js App Router projects.
- [PostHog Event Tracking](https://posthog.com/docs/product-analytics/capture-events) -- How to define and capture events. The concepts here inform your tracking plan.
- [PostHog Funnels](https://posthog.com/docs/product-analytics/funnels) -- How to set up and interpret funnel analyses. Understanding funnels helps you identify where users drop off.
- [PostHog Feature Flags](https://posthog.com/docs/feature-flags) -- How feature flags work, how to create them, and how to evaluate them in code. The "Getting Started" and "Best Practices" sections are most relevant.
- [PostHog Session Replay](https://posthog.com/docs/session-replay) -- How session recording works, what data is captured, and how to configure privacy controls.

---

## Tracking Plans

- [Segment: Tracking Plan Best Practices](https://segment.com/blog/what-is-a-tracking-plan/) -- A conceptual guide to building a tracking plan. The principle of starting with questions, not events, is universally applicable.
- [Amplitude: Data Taxonomy](https://amplitude.com/blog/data-taxonomy-playbook) -- How to organize your analytics events into a coherent taxonomy. Useful for thinking about naming conventions and event structure.

---

## Privacy and Analytics

- [PostHog Privacy Controls](https://posthog.com/docs/privacy) -- PostHog's built-in privacy features, including IP anonymization, session replay masking, and opt-out mechanisms.
- [GDPR for Developers (Smashing Magazine)](https://www.smashingmagazine.com/2018/02/gdpr-for-web-developers/) -- A practical, non-legal overview of what GDPR means for web applications. Relevant if your users include anyone in Europe.
- [CCPA Overview (California AG)](https://oag.ca.gov/privacy/ccpa) -- High-level overview of the California Consumer Privacy Act. Relevant if your users include anyone in California.

---

## Monitoring Philosophy

- [Charity Majors: Observability](https://charity.wtf/2020/03/03/observability-is-a-many-splendored-thing/) -- A thoughtful essay on what observability means and why it matters. Written for engineers but accessible and conceptually valuable.
- [Google SRE Book: Monitoring (Chapter 6)](https://sre.google/sre-book/monitoring-distributed-systems/) -- Google's approach to monitoring. The "Symptoms vs. Causes" section is particularly useful for understanding what to monitor and how to set up useful alerts.

---

## A Note on These Resources

Monitoring and analytics tools have extensive documentation because they're configurable. You don't need to read all of it. Focus on the conceptual overviews -- what each feature does and why it exists. Claude Code handles the integration code. Your job is deciding what to monitor, what to track, what alerts to set up, and what questions to ask of your data.
