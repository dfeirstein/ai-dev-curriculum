# Week 18 Guide: The Speed Build Framework

---

## Part 1: Speed vs. Quality

### The False Trade-off

Most people think speed and quality are opposites -- that going fast means cutting corners. That's true if you don't know what you're doing. It's false when you have strong foundations.

Think about it: a professional chef can prepare a complete meal in 30 minutes. Not because they skip steps. Because they've done it so many times that every movement is efficient, every decision is automatic, and every tool is exactly where they need it.

That's you now. After 17 weeks, you know the stack. You know the workflow. You know the common patterns. The speed comes from eliminating hesitation, not from eliminating quality.

### What You Can Skip vs. What You Can't

**You can skip:**
- Extensive competitive research (brief check is enough)
- Multiple design iterations (pick a clean design and commit)
- Nice-to-have features (ruthlessly cut to core)
- Custom design systems (use shadcn/ui defaults)
- Complex database schemas (simplest possible)

**You cannot skip:**
- Planning your data model (15 minutes of planning saves hours of rework)
- Authentication (if your app needs it)
- Error handling on critical paths
- Basic testing (at least the happy path)
- Monitoring setup (Sentry takes 10 minutes)
- Deployment (an undeployed project is an unfinished project)

The goal is not to skip quality. It's to be ruthlessly efficient about where you spend time.

---

## Part 2: Decision-Making Under Pressure

### Use What You Know

This is not the week to try a new database, a new auth library, or a new deployment platform. Use the stack you know:

- Next.js + TypeScript + Tailwind + shadcn/ui
- Postgres + Drizzle ORM
- Better Auth
- Vercel
- Sentry + PostHog
- GitHub Actions

Every minute spent debugging a new tool is a minute you could have spent building features. Save experimentation for side projects without deadlines.

### Make Decisions and Move On

Speed builders share one trait: they decide quickly. When you face a choice:

1. **Is there an obvious answer?** Take it.
2. **Are both options reasonable?** Pick one. Either will work.
3. **Is the decision reversible?** Then it doesn't matter much. Pick one and keep going.

The only decisions worth deliberating are irreversible ones that affect the core architecture. For a 5-day project, almost nothing is irreversible. Your data model matters. Your button color doesn't. Act accordingly.

### Time-Boxing

Time-boxing means deciding in advance how long you'll spend on something, then stopping when the time is up -- regardless of whether you're "done."

Examples:
- "I'll spend 30 minutes on the landing page. Whatever I have at 30 minutes is good enough."
- "I'll spend 2 hours on the core feature. If it's not working after 2 hours, I'll simplify."
- "I'll spend 15 minutes on this bug. If I can't fix it in 15 minutes, I'll work around it or remove the feature."

Time-boxing prevents the biggest speed killer: getting stuck in a rabbit hole. Two hours debugging a nice-to-have feature while your core isn't tested is a bad trade.

---

## Part 3: The 5-Day Framework

### Day 1: Research + Plan (Whole Day)

This feels counterintuitive. Why spend 20% of your time not building? Because good planning makes Days 2-3 dramatically faster.

**Morning: Choose and research.**
- Pick your project idea (15-30 minutes, max)
- Quick competitive scan (30 minutes -- just see what exists)
- Define your feature set: must-have only (30 minutes)

**Afternoon: Plan the architecture.**
- Data model design with Claude Code
- Page structure mapping
- User flow for the core feature
- Project setup (scaffolding, database, auth)

By end of Day 1, you should have: a working project with auth, database tables created, and basic navigation. No features yet, but the foundation is solid.

### Day 2-3: Build (Core Features Only)

Two days to build all your must-have features. This is where the discipline matters most.

**Rules for the build phase:**
- Build one feature at a time, start to finish
- Test each feature in the browser before moving on
- Commit after each working feature
- Deploy at least once on Day 2 (catch deployment issues early)
- If a feature is taking more than 3 hours, simplify it

No nice-to-have features. No visual polish. No "I'll just add this one more thing." Build the core, verify it works, move on.

### Day 4: Test + Monitor

Apply the quality playbook you know:
- Add Vitest unit tests for core logic
- Add one E2E test for the critical flow
- Set up Sentry error tracking
- Set up PostHog analytics
- Fix any bugs found during testing

This is not the day to write 50 tests. Write the tests that matter most: the ones covering the core feature's happy path and the most likely failure modes.

### Day 5: Polish + Deploy

The final day:
- Run Lighthouse and fix critical issues
- Add loading states, error states, and empty states
- Mobile responsiveness check
- Write the README
- Final deployment
- Take screenshots

By end of Day 5, the app is live, documented, and ready to show.

---

## Part 4: Common Speed Build Mistakes

### Starting Without a Plan

"I'll figure it out as I go" is the fastest way to waste two days and then panic. Even 2 hours of planning on Day 1 saves you from major restructuring on Day 3.

### Scope Creep Mid-Build

You're building the core feature and suddenly think: "It would be really cool if it also did [X]." Stop. Write it down on a "later" list. Keep building the core. You can always add features after the sprint if you want. You can't get back the time you spent on a tangent.

### Polishing Too Early

Your landing page looks decent but not perfect. You spend 2 hours tweaking colors and spacing. Meanwhile, your core feature isn't tested and monitoring isn't set up. Polish is Day 5 work. Don't let it creep into Days 2-3.

### Not Deploying Until Day 5

Deployment issues discovered on Day 5 can kill your timeline. Deploy on Day 2. Find and fix environment issues early, when you have time to deal with them.

---

## Part 5: After the Sprint

### What You've Proven

When you ship this project, you'll have proven something significant: you can go from zero to a production-quality application in 5 days. That's not a trivial skill. That's the skill that:

- Lets freelancers take on more clients
- Lets startup founders test ideas quickly
- Lets job candidates build impressive portfolios
- Lets anyone turn an idea into reality, fast

### Your Speed Will Only Increase

This is your first speed build. It will feel intense. But next time, it'll be faster. And the time after that, even faster. The patterns become automatic. The decision-making becomes instant. The workflow becomes second nature.

By the time you're building your fifth or tenth project, the 5-day framework won't feel like a constraint. It'll feel like plenty of time.
