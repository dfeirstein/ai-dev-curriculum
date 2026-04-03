# Week 1: Your AI Development Environment — Instructor Guide

## Week at a Glance
- Theme and key concepts: First contact with Claude Code CLI and the deploy-from-day-one philosophy. Students go from zero tools to a live personal landing page without writing a single line of code.
- What students ship: A personal landing page deployed to a live Vercel URL
- Estimated hours: 4-6
- Difficulty: 2

## Pacing Guide
- **Day 1-2:** Environment setup (Ghostty, Node.js, Claude Code CLI, GitHub, Vercel). Read guide.md. First Claude Code interaction — have them ask Claude to explain what it can do. Create project folder and initialize the repo.
- **Day 3-4:** Build the landing page through Claude Code. Iterate on design, content, and layout. Practice the feedback loop: prompt, review, refine.
- **Day 5:** Final polish, deploy to Vercel, verify the live URL works. Share the link.

## Getting Students Started
- Kick off by showing them a finished example — a simple, clean landing page at a real URL. Make the end state concrete before they touch any tools.
- The most common confusion at the start is "what am I supposed to type?" Reassure them that there is no wrong first prompt. A good starter: "Claude, create a personal landing page with my name, a short bio, and links to my GitHub and LinkedIn."
- Students should have open: Ghostty terminal and a web browser side by side. Nothing else.

## Check-in Points
- **After guide.md:** Verify they can explain what Claude Code is (not a code editor, not ChatGPT — a CLI agent that reads and writes files). Verify they understand they are directing, not coding.
- **Mid-project checkpoint:** They should have a working local page they can see in the browser. If they don't have `npx create-next-app` or equivalent scaffolded yet, they are behind. Help them get Claude to scaffold the project.
- **Pre-ship review:** The page loads locally without errors. They have a GitHub repo. They can explain what Vercel does (hosts the site, auto-deploys from GitHub).

## Common Pitfalls
- **Environment setup eats the whole week.** Node version mismatches, PATH issues, Ghostty config. Budget extra time for Day 1. Walk the room. If someone is stuck on install for more than 20 minutes, pair with them directly.
- **Students try to write code by hand.** Old habits. If you see them editing files in VS Code or typing HTML, redirect them. "Close that editor. Tell Claude what you want instead."
- **Vercel deployment fails silently.** Usually a missing `package.json` or wrong framework preset. Have them check the Vercel build logs. Common fix: Claude forgot to set the framework to Next.js in Vercel settings.
- **Students accept Claude's first output without reviewing.** They see a page and say "done." Push them to actually read what's on screen. Does the bio sound like them? Are the links correct? This sets the habit for every future week.
- **Git confusion.** Students who have never used Git will struggle with init, add, commit, push. Have Claude handle all Git operations, but make sure students can explain what "commit" and "push" mean at a basic level.

## How to Know the Student Gets It
- They can open Claude Code, give it a natural-language instruction, and review the output critically — not just accept it.
- They can deploy a change: edit via Claude, see it locally, push to GitHub, watch Vercel redeploy.
- They describe the workflow as "I tell Claude what I want, I check the result, I give feedback" rather than "Claude builds it for me."
- Red flags: Student cannot explain what any file in their project does. Student has a live site but cannot make a change to it. Student is copying code from somewhere and pasting it into files.

## Support Strategies
- If stuck on environment setup: Don't let them spiral. Sit with them, check Node version (`node -v`), check that Claude Code is installed (`claude --version`). Most issues are PATH-related.
- If rushing without reviewing: Ask them to read you the bio text on their landing page out loud. Ask "Is that what you want it to say?" This builds the review habit.
- If frustrated: Remind them this is the hardest week in terms of setup-to-payoff ratio. Once the environment works, every future week starts faster. Show them someone else's working deploy to keep motivation up.

## Differentiation
- **Moving fast?** Add a custom favicon, an animated gradient background, or a dark/light mode toggle. Have them direct Claude to add a "projects" section with placeholder cards.
- **Struggling?** Reduce scope to just a name, one sentence, and one link. The goal is a live URL, not a beautiful page. They can improve it later.
- **Minimum viable ship:** A page with their name on it, deployed to a URL that loads in a browser. That is the only hard requirement to move to Week 2.
