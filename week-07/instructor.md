# Week 7: Portfolio + Blog — Instructor Guide

## Week at a Glance
- Theme and key concepts: First independent build. Students drive the decisions — layout, content, scope, style — with less hand-holding from the project guide. They build a portfolio site with MDX blog, learning to make and commit to design decisions instead of endlessly iterating.
- What students ship: A personal portfolio with project showcases, an about page, a blog with at least one MDX post, and SEO basics — deployed live
- Estimated hours: 10-12
- Difficulty: 3

## Pacing Guide
- **Day 1-2:** Research phase. Study 5-10 real portfolios. Make design decisions: layout style, color palette, typography, number of pages. Write a brief (even 3 bullet points) before building anything. Set up the project scaffold with Next.js, Tailwind, shadcn/ui.
- **Day 3-4:** Build core pages: Home/hero, About, Projects showcase, Contact. Set up MDX for the blog. Write at least one real blog post (not lorem ipsum — something genuine, even if short).
- **Day 5:** SEO metadata, social sharing images, performance check, final polish. Deploy. Share the URL.

## Getting Students Started
- Kick off with a portfolio review session. Show 3-4 portfolios of varying quality. Ask: "What do you notice first? What makes you stay? What makes you leave?" This frames the build in terms of user experience, not technical features.
- Common confusion: decision paralysis. Students want to look at 50 portfolios and find the "perfect" design before starting. Set a timebox: 45 minutes of research, then you must commit to a direction. They can change it later, but they must start.
- Students should have open: Ghostty terminal, browser with 2-3 portfolio examples they liked, and a list of content they want to include (projects, bio, links).

## Check-in Points
- **After guide.md:** Does the student have a written brief — even informal — describing what their portfolio will include and what vibe they are going for? If they say "I'll figure it out as I go," push them to commit to at least: number of pages, one adjective describing the visual style (minimal, bold, playful), and what projects they will showcase.
- **Mid-project checkpoint:** At least 3 pages are built and the blog infrastructure (MDX) is working. If MDX is not set up by mid-week, prioritize it now — it is the most technically tricky part. If the student is still redesigning the home page for the fourth time, intervene on scope.
- **Pre-ship review:** All pages load and link correctly. Blog post renders from MDX (not hardcoded HTML). The site has real content — the student's actual name, actual project descriptions, actual writing. Deployed to a live URL.

## Common Pitfalls
- **Decision paralysis on design.** Student cannot choose between layouts, color schemes, fonts. They redesign the hero section five times. Intervention: force a decision. "Pick one. Ship it. You can redesign next month. The goal is a live portfolio, not a perfect one."
- **Scope creep.** Student wants animations, a dark/light mode toggle, a newsletter signup, a reading time estimator, a custom cursor, and parallax scrolling — all in one week. Help them prioritize ruthlessly. What is the smallest version that represents them well?
- **MDX confusion.** Students do not understand what MDX is or why it exists. Explain simply: it is Markdown that can include React components. Write in Markdown, but drop in a custom callout or code block component when needed. The setup (configuring `next-mdx-remote` or `contentlayer` or similar) is the hard part — once it works, writing posts is easy.
- **Placeholder content everywhere.** "Project 1 - Description coming soon." This defeats the purpose. Push students to write real content even if it is short. Three sentences about a real project beats a beautiful card with lorem ipsum.
- **Ignoring mobile.** Portfolio looks great on the student's laptop, broken on a phone. Have them resize the browser to 375px width before shipping. Navigation, text size, and images should all work at mobile width.

## How to Know the Student Gets It
- They made design decisions and can explain why. "I went minimal because I want the projects to be the focus" shows understanding. "I just asked Claude to make it look good" does not.
- They drove the build. In previous weeks, the project guide told them what to do step by step. This week, they decided the pages, the layout, the content structure. They directed Claude with their own vision, not just following instructions.
- Their portfolio has real content that represents them as a person. It feels like theirs, not like a template.
- Red flags: Portfolio is identical to a template with only the name changed. Student deferred every decision to Claude ("Claude, what should my portfolio look like?"). No blog post exists or it is lorem ipsum. Student spent the entire week on the home page and shipped nothing else.

## Support Strategies
- If stuck on design decisions: Give them constraints. "You have 30 minutes to pick a color palette. Use coolors.co, pick one, and move on." Constraints free people from paralysis.
- If rushing without reviewing: Ask them to read their About page out loud. Ask if that is how they would introduce themselves to someone they respect. Personal content requires personal review.
- If frustrated: Usually frustrated because this is the first week where the project guide does not give step-by-step instructions. Validate that independence is harder. Offer to brainstorm with them for 5 minutes, then step back and let them drive.

## Differentiation
- **Moving fast?** Add a project detail page for each project (not just cards, but full case studies). Add RSS feed for the blog. Add reading time estimates. Add a "uses" page (tools/setup they use). Animate page transitions.
- **Struggling?** Reduce to Home + About + one blog post. Skip the Projects showcase and Contact page. A portfolio with real content on two pages beats an ambitious five-page site that is half-built.
- **Minimum viable ship:** A live portfolio with at least a home page, one other page, and one blog post rendered from MDX. Real content — their name, their words, their projects. Deployed to a URL they can share.
