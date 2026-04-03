# Week 11 Resources

Reference material for dashboards, data visualization, admin panels, and data export. Focus on understanding the concepts -- not implementation details.

---

## Data Visualization

- [Recharts](https://recharts.org/) -- A React charting library built on D3. Clean API, good defaults, works well with Next.js. Browse the examples gallery for inspiration.
- [Chart.js](https://www.chartjs.org/) -- Another popular charting library. The "Getting Started" page gives a good conceptual overview of chart types and when to use them.
- [Data Visualization Catalogue](https://datavizcatalogue.com/) -- A reference for different chart types. For each chart, it explains what it's for and when to use it. Great for deciding which visualization fits your data.
- [Storytelling with Data](https://www.storytellingwithdata.com/chart-guide) -- A guide to choosing the right chart. Focuses on communication, not just aesthetics.

---

## Dashboard Design

- [Refactoring UI: Design Tips](https://www.refactoringui.com/) -- Practical design advice from the creators of Tailwind CSS. The dashboard-related tips about hierarchy, spacing, and layout are directly applicable.
- [Dashboard Design Patterns (UX Planet)](https://uxplanet.org/dashboard-design-best-practices-and-examples-82decdb0c53f) -- Common patterns for organizing dashboard layouts, choosing metrics, and designing for scannability.
- [shadcn/ui Charts](https://ui.shadcn.com/charts) -- If you're using shadcn/ui, their chart components are built on Recharts and come with sensible defaults.

---

## Admin Panels

- [shadcn/ui Data Table](https://ui.shadcn.com/docs/components/data-table) -- A guide to building data tables with shadcn/ui. Member management tables, activity logs, and export previews all use this pattern.
- [Stripe Customer Portal](https://stripe.com/docs/customer-management/portal-deep-dive) -- How the Stripe-hosted customer portal works. Your admin panel links to this for subscription management.

---

## Activity Logs and Audit Trails

- [Audit Logging Best Practices](https://www.datadoghq.com/knowledge-center/audit-logging/) -- A conceptual overview of what to log, how to structure log entries, and why audit trails matter. Focused on the "what" and "why," not implementation.

---

## Data Export

- [CSV on Wikipedia](https://en.wikipedia.org/wiki/Comma-separated_values) -- If you want to understand the CSV format itself. Simple but worth knowing the edge cases (what happens when data contains commas or newlines).
- [MDN: Blob and File APIs](https://developer.mozilla.org/en-US/docs/Web/API/Blob) -- Conceptual reference for how browsers handle file downloads. Claude Code uses these APIs when building the export feature.

---

## The v1 Mindset

- [Ship It (Seth Godin)](https://seths.blog/2010/06/fear-of-shipping/) -- A short, powerful essay on why shipping matters more than perfecting.
- [The Lean Startup: MVP](https://theleanstartup.com/principles) -- The original articulation of "minimum viable product." The core idea: build the smallest thing that delivers value, ship it, and learn from real users.

---

## A Note on These Resources

You'll notice we link to charting libraries and dashboard design resources, not charting tutorials. You won't configure chart axes or write data transformation code. Claude Code does that. But understanding what chart types exist, when each one is appropriate, and what makes a dashboard scannable helps you give Claude Code better direction -- and evaluate whether the result actually communicates the data effectively.
