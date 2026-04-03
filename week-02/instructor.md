# Week 2: The Claude Code Workflow — Instructor Guide

## Week at a Glance
- Theme and key concepts: The research-plan-execute workflow and why skipping steps leads to bad output. Students learn CLAUDE.md, Git branching, and how to critically review Claude's work before committing.
- What students ship: A multi-page personal website (Home, About, Projects, Blog placeholder, Contact) with consistent navigation and responsive design
- Estimated hours: 6-8
- Difficulty: 3

## Pacing Guide
- **Day 1-2:** Read guide.md. Introduce the research-plan-execute framework. Students create their CLAUDE.md and practice the research phase — asking Claude questions about site architecture before building anything.
- **Day 3-4:** Build pages one at a time, each following the full workflow. Practice Git branching: one branch per page, review diff before merging. This is where the habits form.
- **Day 5:** Polish navigation, responsive design, consistency across pages. Deploy updated site. Verify all pages work at the live URL.

## Getting Students Started
- Kick off by showing what happens when you skip research: give Claude a vague prompt like "build me a website" and show the mediocre result. Then show what happens when you research first, write a plan, and execute step by step. The difference is the lesson.
- Common confusion: students think CLAUDE.md is documentation for humans. Clarify that it is instructions for Claude — it shapes every response Claude gives in that project.
- Students should have open: Ghostty terminal, browser with their Week 1 site loaded, and the guide.md for reference.

## Check-in Points
- **After guide.md:** Can the student explain the three phases (research, plan, execute) and why the order matters? Can they explain what CLAUDE.md does? If they say "it's just a readme," they need more context.
- **Mid-project checkpoint:** They should have at least 2 pages built on separate branches, merged into main. Check their Git log — if everything is one giant commit on main, stop and teach branching now. Also check their CLAUDE.md — if it is empty or generic boilerplate, have them flesh it out.
- **Pre-ship review:** All pages load. Navigation works on every page. The site is responsive (check by resizing the browser). Design is consistent — same fonts, colors, spacing across pages.

## Common Pitfalls
- **Jumping straight to "Claude, build me an About page."** This is the most common mistake in Week 2. The student skips research and planning entirely. When you see this, ask: "What did you learn in the research phase?" If they can't answer, send them back to research.
- **Not reviewing Claude's output.** Student prompts Claude, sees files change, commits, moves on. They don't read the code, don't check the browser, don't verify anything. Intervention: ask them to show you the diff before they commit. Ask "what changed and why?"
- **Git branch confusion.** Students create a branch but keep working on main, or they don't know how to merge. Have Claude handle the Git commands, but make sure the student can tell you what branch they are on and what happens when they merge.
- **CLAUDE.md is too vague.** "Use good practices" is not a useful instruction. Push students to be specific: "Use Tailwind for all styling. No inline styles. Mobile-first responsive design. All components in /components."
- **Scope creep on individual pages.** A student spends 3 hours perfecting the About page animations while the Blog and Contact pages don't exist. Enforce the rule: get all pages to "good enough" before perfecting any one page.

## How to Know the Student Gets It
- Before building anything, they ask Claude research questions and read the answers. They can summarize what they learned.
- They create a branch, build a feature, review the diff, and merge — without being told to. The workflow is becoming muscle memory.
- When Claude produces something unexpected, they give specific feedback ("The nav should be sticky, not static" vs "fix it").
- Red flags: Every change is on the main branch. CLAUDE.md is still the default from Week 1. Student cannot explain what any of the files Claude created actually do. Student says "Claude just does it" when asked about the process.

## Support Strategies
- If stuck on the workflow: Pair with them on one page. You narrate the phases out loud: "First, let's research. Now let's plan. Now let's execute." Walk through one full cycle together, then have them do the next page solo.
- If rushing without reviewing: Institute a "show me the diff" rule. Before every commit, the student must run `git diff` and explain one thing that changed. This slows them down productively.
- If frustrated: Usually they are frustrated because Claude "isn't listening." Check their CLAUDE.md — often the fix is better project context. Also check if their prompts are vague. Help them rewrite one prompt to be specific and watch the improvement.

## Differentiation
- **Moving fast?** Add a working contact form that validates input (client-side). Add page transitions. Have them write a real blog post as a static page and style it well.
- **Struggling?** Reduce to 3 pages (Home, About, Contact). Skip the Blog and Projects pages. Focus on getting the workflow right on fewer pages rather than rushing through all five.
- **Minimum viable ship:** Three or more pages with working navigation, deployed to a live URL. Evidence of at least one Git branch in their history (not everything on main). A CLAUDE.md that has project-specific content.
