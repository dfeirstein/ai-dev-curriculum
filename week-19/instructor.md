# Week 19: Portfolio Assembly — Instructor Guide

## Week at a Glance
- Theme: Packaging everything the student has built into a professional portfolio. Polishing READMEs, capturing screenshots and demo videos, building a portfolio site, and preparing to present their work to the world.
- What students ship: A deployed portfolio site linking to all projects (TeamTask Pro, Project 3, Speed Build, and optionally the npm package/open source contribution). Each project has a polished README with screenshots. At least one project has a demo video.
- Estimated hours: 8-12
- Difficulty: 2

## Pacing Guide
- **Day 1-2:** Audit and polish existing project READMEs. Take screenshots of each project in its best state (dashboard with real data, key features visible, mobile views). Record a 60-90 second demo video for their strongest project using a screen recorder.
- **Day 3-4:** Build the portfolio site. Direct Claude Code to create a clean, simple portfolio using Next.js (or a static site if they prefer). The portfolio should have: a brief bio, project cards with screenshots and links, a tech stack summary, and contact info. Deploy to Vercel.
- **Day 5:** Final polish pass on everything. Cross-link projects to the portfolio. Review all deployed URLs to make sure they work. Get feedback from a peer or the instructor. Ship.

## Getting Students Started
- This week feels easy compared to recent weeks, but students consistently underestimate it. Polishing is tedious and takes longer than expected. Set expectations early: "This is not a light week. Making your work presentable is real work."
- Common confusion: students do not know what a good developer portfolio looks like. Show them 2-3 examples of strong portfolios (simple, clean, project-focused) before they start. The portfolio does not need to be flashy -- it needs to clearly communicate what they built and what they can do.
- Students should have open: Claude Code CLI, all project repos, all deployed app URLs, a screen recording tool (QuickTime on Mac, OBS, or Loom), and a browser for reference portfolios.

## Check-in Points
- **After guide.md:** Verify the student has a list of all projects they will include and has identified which project is their "hero" project (the one they are most proud of, which gets the most prominent placement and the demo video).
- **Mid-project checkpoint:** By end of Day 3, all project READMEs should be updated and the portfolio site should be scaffolded with placeholder content. If the student is behind on READMEs, have them focus on their top 2 projects only. A portfolio with 2 polished projects is better than 4 rough ones.
- **Pre-ship review:** Every link works (deployed apps, GitHub repos, portfolio site). Screenshots are clear and show the app in a good state. The demo video is watchable (no 5-minute rambling -- 60-90 seconds, focused). The portfolio site loads fast and looks professional on both desktop and mobile.

## Common Pitfalls
- **Underestimating README polish.** Students think they can update 4 READMEs in an hour. A good README takes 1-2 hours: write the description, curate screenshots, document the tech stack, add setup instructions, proof-read. Budget accordingly.
- **Screenshots of empty or broken states.** Students take screenshots of their app with no data or with visible errors. Before screenshotting, populate the app with realistic demo data. The screenshot should show the app at its best.
- **Demo video is too long or too unfocused.** The demo should be scripted (even loosely): "This is [app name]. It does [thing]. Let me show you [core flow]." Practice once before recording. Cut anything over 90 seconds.
- **Over-designing the portfolio site.** Students spend 3 days on the portfolio site design and run out of time for README polish. The portfolio is a container for the projects -- simple and clean wins. A grid of project cards with screenshots, descriptions, and links is sufficient.
- **Dead links.** Deployed apps from earlier weeks may have expired free tier resources (database, hosting). Check every link before shipping. If something is down, redeploy it or note it and provide the GitHub link instead.

## How to Know the Student Gets It
- The student treats portfolio work as seriously as feature work -- it is not an afterthought.
- The student can pitch each project in 2 sentences: what it is and what makes it interesting.
- The student asks for feedback on their portfolio before considering it done.
- The portfolio tells a story: "I started here, I built these things, this is what I can do."
- **Red flags:** Student's portfolio has Lorem Ipsum text. Student's READMEs still say "This is a [Next.js](https://nextjs.org) project bootstrapped with create-next-app." Student has not checked if their deployed apps still work. Student spent 4 days on portfolio site animations and zero time on READMEs.

## Support Strategies
- Provide a README template: project name, one-line description, screenshot, "Built with" tech stack badges, key features list (3-5 bullets), what you learned, how to run locally. This eliminates the blank page problem.
- For the demo video, offer to watch their first take and give feedback. Most students need 2-3 takes. The first is always too long.
- If a deployed app is down and cannot be easily redeployed, help the student take good screenshots and include them in the README. A well-documented project with screenshots is almost as good as a live demo.
- Review the portfolio together on Day 5. Look at it as a hiring manager would: is it clear what this person can do? Can I see their work? Do the links work?

## Differentiation
- **Moving fast?** Add a blog section to the portfolio with a "what I learned" post about their favorite project. Add case study pages for each project (problem, approach, technical decisions, outcome). Set up a custom domain. Add subtle animations and transitions.
- **Struggling?** Cut the portfolio site. Focus entirely on polishing the top 2 project READMEs with good screenshots. A strong GitHub profile with polished repos is a valid portfolio. The portfolio site can be built later.
- **Minimum viable ship:** 2 project READMEs with descriptions and screenshots, all deployed URLs working, and either a portfolio site or a polished GitHub profile page that links to the projects.
