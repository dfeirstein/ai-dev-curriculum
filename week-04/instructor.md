# Week 4: The Modern Web Stack — Instructor Guide

## Week at a Glance
- Theme and key concepts: Understanding what Next.js, TypeScript, Tailwind CSS, and shadcn/ui actually do and why they exist. Students do not need to master any of these — they need to understand what each tool is responsible for so they can direct Claude effectively.
- What students ship: A rebuilt personal website using Next.js App Router, TypeScript strict mode, Tailwind CSS, and shadcn/ui — with dark mode and responsive design, deployed to Vercel
- Estimated hours: 6-8
- Difficulty: 2

## Pacing Guide
- **Day 1-2:** Read guide.md. Research phase: have Claude explain each tool and what problem it solves. Scaffold a fresh Next.js project with TypeScript, Tailwind, and shadcn/ui. Get the dev server running.
- **Day 3-4:** Rebuild the personal site pages using proper components and the full stack. Focus on shadcn/ui components for buttons, cards, navigation. Add dark mode. Responsive design.
- **Day 5:** Polish, accessibility check, Lighthouse score review, deploy. Compare the new site to the Week 2 version — articulate what improved and why.

## Getting Students Started
- Kick off by naming the overwhelm: "There are four new tools this week and you don't need to master any of them." The goal is understanding what each one does, not becoming an expert. They direct Claude to use these tools — their job is knowing when to say "use a shadcn Card component here" vs "add a Tailwind gradient."
- Common confusion: students think they need to learn TypeScript syntax. They don't. They need to understand that TypeScript catches errors before runtime. When Claude writes TypeScript and there are red squiggles, that is TypeScript doing its job.
- Students should have open: Ghostty terminal, browser with the dev server running (`localhost:3000`), and their Week 2 site in another tab for reference.

## Check-in Points
- **After guide.md:** Can the student explain, in one sentence each: What does Next.js do? (Routing, server-side rendering, API routes.) What does TypeScript add? (Type checking — catches errors before you run the code.) What does Tailwind do? (Styling with utility classes instead of CSS files.) What does shadcn/ui do? (Pre-built components you own and can customize.) If they cannot distinguish these four, spend more time on research.
- **Mid-project checkpoint:** The scaffold is up, dev server runs, at least 2 pages are rebuilt with shadcn/ui components. If they are still on the scaffold step, check for errors in the terminal — usually a missing dependency or Node version issue.
- **Pre-ship review:** All pages work. Dark mode toggle functions. Site is responsive at mobile, tablet, and desktop widths. No TypeScript errors (`npx tsc --noEmit` passes). Lighthouse performance score is reasonable (above 80).

## Common Pitfalls
- **Getting overwhelmed by the stack.** Student freezes because there are too many new concepts. Intervention: focus on one tool at a time. "Forget about everything else. Right now, we are just talking about what shadcn/ui components look like." Sequence the understanding.
- **Trying to learn TypeScript syntax.** Student opens TypeScript documentation and starts reading about generics. Pull them back. They do not write TypeScript — Claude does. They need to know what TypeScript errors mean, not how to write type annotations.
- **Ignoring shadcn/ui and hand-building everything.** Claude might generate raw HTML+Tailwind instead of using shadcn components. Students should direct Claude to use shadcn/ui: "Use a shadcn Card for each project. Use the shadcn Button component for the CTA."
- **Not understanding App Router vs Pages Router.** If Claude generates a `pages/` directory, the student is on the old pattern. Make sure the project uses the `app/` directory with `layout.tsx` and `page.tsx` files.
- **Dark mode doesn't actually work.** Common issue where the toggle exists but the theme does not persist or the colors don't change. Have Claude check that `next-themes` or the shadcn dark mode setup is properly configured in `layout.tsx`.

## How to Know the Student Gets It
- They can look at a page and tell you which parts are shadcn/ui components, which parts are Tailwind styling, and which behavior comes from Next.js (like navigation between pages without a full reload).
- When something looks wrong, they can identify which tool is responsible. "The button style is off" = Tailwind or shadcn issue. "The page doesn't load" = Next.js routing issue. "The data type is wrong" = TypeScript issue.
- They can direct Claude using stack-specific vocabulary: "Use the shadcn Sheet component for mobile nav" rather than "make a sidebar thing."
- Red flags: Student cannot explain what any of the four tools does. The rebuilt site is identical to Week 2 with no shadcn components. Student is manually editing `.tsx` files.

## Support Strategies
- If stuck on the stack concepts: Use the "restaurant kitchen" analogy. Next.js is the kitchen layout (where things go, how orders flow). TypeScript is the health inspector (catches problems before customers see them). Tailwind is the spice rack (styling ingredients you combine). shadcn/ui is the pre-made sauces (components ready to use and customize).
- If rushing without reviewing: Ask them to toggle dark mode and resize the browser to mobile. These two checks catch most quality issues and take 10 seconds.
- If frustrated: This week is intentionally lighter on difficulty. If a student is struggling, the issue is likely conceptual overwhelm, not technical difficulty. Slow down the research phase. It is fine to spend 2 full days on understanding before building.

## Differentiation
- **Moving fast?** Add page transitions with Framer Motion. Implement a command palette (Cmd+K) using shadcn/ui's Command component. Add SEO metadata to every page.
- **Struggling?** Rebuild just 2 pages (Home and About) instead of the full site. Focus on getting one page right with shadcn components and dark mode before expanding.
- **Minimum viable ship:** A Next.js site with at least 2 pages using shadcn/ui components, dark mode working, deployed to Vercel. The student can name the four stack tools and explain what each one does.
