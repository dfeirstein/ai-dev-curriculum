# Instructor Guide

> Read this in 30 minutes. Facilitate with confidence for 20 weeks.

---

## Table of Contents

1. [Your Role as Instructor](#1-your-role-as-instructor)
2. [Course Philosophy Recap](#2-course-philosophy-recap)
3. [How to Pace a Cohort](#3-how-to-pace-a-cohort)
4. [The Check-in Rhythm](#4-the-check-in-rhythm)
5. [Evaluating Student Progress](#5-evaluating-student-progress)
6. [When to Intervene vs Let Them Struggle](#6-when-to-intervene-vs-let-them-struggle)
7. [Using Claude Code as a Teaching Tool](#7-using-claude-code-as-a-teaching-tool)
8. [Phase Transition Notes](#8-phase-transition-notes)
9. [Common Student Patterns](#9-common-student-patterns)
10. [Week-by-Week Overview Table](#10-week-by-week-overview-table)

---

## 1. Your Role as Instructor

You are not a lecturer. You are a facilitator.

Students in this curriculum learn by doing. They direct Claude Code to build real software, deploy it to production, and verify it works. Your job is not to explain how JavaScript closures work or walk through a React tutorial on a whiteboard. Your job is to:

- **Unblock.** When a student is stuck, figure out whether it's a productive struggle (they need to work through it) or an unproductive one (environment issue, circular error, missing credential). Act accordingly.
- **Guide.** When a student is heading in the wrong direction -- skipping the research phase, accepting broken output from Claude, building the wrong thing -- nudge them back on track.
- **Ensure they ship.** Every week has a deliverable. Your primary metric is: did they ship it? Is it live? Does it work?

Think of yourself as a film school instructor. You are not teaching students to operate cameras. You are teaching them to be directors -- to understand what they want, communicate it clearly, and evaluate whether the result matches their vision. The AI is the crew. You are the mentor who helps them develop the director's eye.

**What you teach:**

- The builder mindset: research, plan, execute, verify, ship
- How to evaluate whether something works (quality gates, not code reading)
- How to communicate clearly with an AI agent
- How to make architectural and product decisions
- When to ship and when to keep iterating

**What you do NOT teach:**

- How to write code by hand
- Syntax, language features, or framework internals
- Traditional CS concepts (unless a student asks, and then you point them to Claude)

If a student asks you a technical question, the correct response is often: "Ask Claude Code. Use plan mode. Then tell me what you learned." This is not laziness -- it is the core skill of the curriculum.

---

## 2. Course Philosophy Recap

The foundational belief of this curriculum: **you don't need to write code to build software.** Students direct Claude Code CLI to research, plan, and build. They never type a line of code by hand. Their job is to know *what* to build, *why* each tool exists, and *how* to guide the AI to produce production-quality output.

The workflow every student should internalize:

1. **Research** -- Understand the problem and the tools
2. **Plan** -- Design the solution and architecture
3. **Execute** -- Direct Claude Code to build it
4. **Verify** -- Check the output with quality gates (lint, type-check, test, build)
5. **Ship** -- Deploy to production

Quality gates are non-negotiable. Claude Code generates code. Quality gates verify it. Students should never evaluate code by reading it line-by-line. They evaluate it by running lint, type-check, test, and build -- and by using the deployed application. If it passes the gates and works in the browser, it ships.

The analogy that works: filmmaking. Traditional bootcamps teach you to operate a camera. This curriculum teaches you to be a director. You understand cinematography well enough to give clear direction, but you are not operating the camera yourself.

Reinforce this constantly. Students who internalize this mental model move fast. Students who resist it (trying to read and understand every line of generated code) fall behind.

---

## 3. How to Pace a Cohort

### Schedule Options

**5-day week (recommended for cohort-based):**
- Monday through Friday
- Weekends off
- 20 calendar weeks = ~5 months

**7-day week (accelerated):**
- Every day
- Students manage their own rest
- 20 calendar weeks but with more daily flexibility
- Best for self-motivated adult learners

**Self-paced:**
- Students move at their own speed
- Set a maximum timeline (30 weeks recommended) to prevent stalling
- Requires more check-in discipline from you

### Recommended Daily Structure (Cohort-Based, 5-Day Week)

| Time | Activity |
|------|----------|
| First 15 min | Async standup (Slack/Discord post) or live standup |
| Morning block | Guided work: reading guide.md, research, planning |
| Midday | Lunch / break |
| Afternoon block | Hands-on building with Claude Code |
| Last 15 min | End-of-day log: what shipped, what's stuck, what's next |

Total daily commitment: 2-3 hours on light weeks, 3-4 hours on heavy weeks.

### Handling Different Speeds

Students will move at different speeds. This is fine. Here is how to handle it:

- **Fast students:** Give them the stretch goals from each project.md. Point them to the resources.md for deeper dives. Let them help slower students (pair directing is a real skill). Do NOT let them skip ahead to the next week -- depth beats speed.
- **Average students:** They should finish the core deliverable by end of week. If they are consistently finishing mid-week, the stretch goals are there for a reason.
- **Slow students:** Identify the bottleneck. Is it environment issues? Prompting skill? Conceptual understanding? Fear of breaking things? Each has a different fix. The most common bottleneck in Phase 1 is environment setup. The most common bottleneck in Phase 2 is conceptual overload. The most common bottleneck in Phase 3 is scope creep.

If a student falls more than one week behind, schedule a 1:1. Do not let the gap widen silently.

---

## 4. The Check-in Rhythm

### Daily Standup (5-10 minutes, async or sync)

Every student answers three questions:

1. **What did I ship yesterday?** (Not "what did I work on" -- what did I ship?)
2. **What am I building today?**
3. **Am I stuck on anything?**

If async (Slack/Discord), students post by a set time each morning. You read every post. Respond to anyone who is stuck. Flag anyone who has not posted in two days.

If sync (video call or in-person), keep it to 5-10 minutes. No problem-solving in standup -- just status. Move problem-solving to 1:1s.

### Mid-Week Review (Wednesday, 15-30 minutes)

Check in on progress toward the weekly deliverable. Ask:

- "Show me what you have so far."
- "What's left to ship by Friday?"
- "What's the hardest part remaining?"

This is a course-correction opportunity. If someone is off-track, Wednesday is early enough to fix it. Friday is not.

### End-of-Week Ship Review (Friday, 30-60 minutes)

This is the most important check-in of the week. Each student demonstrates what they shipped.

Format: each student gets 3-5 minutes to:
1. Show the live deployed application
2. Walk through what it does
3. Explain one decision they made and why
4. Mention one thing they learned

**What to evaluate:**
- Is it deployed and live? (Non-negotiable)
- Does the core functionality work?
- Can they explain what they built?
- Did they follow the research-plan-execute workflow?

**What NOT to evaluate:**
- Code quality (that is what quality gates are for)
- Visual design perfection (unless it is a design-focused week)
- Whether they did it "the right way" (there are many right ways)

Ship reviews should feel like a celebration, not an exam. Students are building real things every week. Acknowledge that.

---

## 5. Evaluating Student Progress

### The Rubric

There are three questions. They are in priority order.

**1. Did they ship?**
Is there a live URL? Can you click on it and use the application? This is binary. Yes or no.

**2. Does it work?**
Does the core functionality described in the acceptance criteria actually function? Can you create an account, add a task, see the dashboard? Test it like a user, not an engineer.

**3. Can they articulate what they built and why?**
Ask them to explain the project without looking at any notes. What does it do? What tools did they use? Why those tools? What was the hardest part? If they can explain it clearly, they understand it. If they cannot, they may have blindly accepted Claude's output without understanding the decisions.

### Grading (If Required)

If you need formal grades:

| Grade | Criteria |
|-------|----------|
| **A** | Shipped on time. Works fully. Can explain decisions clearly. Completed stretch goals. |
| **B** | Shipped on time. Core functionality works. Can explain most decisions. |
| **C** | Shipped late or partially. Some functionality works. Struggles to explain decisions. |
| **D** | Did not ship. Significant functionality missing. Cannot articulate what was built. |
| **F** | Did not attempt or submit. |

### Red Flags

Watch for these patterns -- they indicate a student is falling behind in a way that will compound:

- **Cannot explain what a tool does.** If a student uses Drizzle all week but cannot tell you what an ORM is, they are copying without understanding. Have them ask Claude Code to explain.
- **Quality gates never pass.** If lint, type-check, or build is constantly failing and they are ignoring it, they have not internalized the verify step. Sit with them and run the gates together.
- **Ships but never deployed.** "It works on my machine" is not shipping. Deployment is part of every week. If they are skipping it, they are avoiding the hardest part.
- **Stops posting standups.** Radio silence is the earliest warning sign. Reach out immediately.
- **Only works during check-ins.** If all their commits happen in the hour before ship review, they are cramming, not learning the workflow.

---

## 6. When to Intervene vs Let Them Struggle

This is the hardest judgment call you will make. Here is a framework.

### Productive Struggle (Let Them Work Through It)

The student is:
- Learning to write better prompts after Claude misunderstands them
- Debugging with Claude Code (asking it to explain errors, try different approaches)
- Figuring out how to describe what they want more precisely
- Reading error messages and feeding them back to Claude
- Iterating on a design that does not look right yet
- Working through the research phase and feeling overwhelmed by options

**Your response:** Check in. Ask "what have you tried?" If they are making progress (even slow progress), let them continue. Productive struggle is where the deepest learning happens. A student who spends 45 minutes learning to prompt Claude through a confusing error has learned more than one who got the answer from you in 30 seconds.

### Unproductive Struggle (Intervene)

The student is:
- Stuck on environment setup (Node version, missing dependency, credential issue)
- In a circular error loop (same error, same fix attempt, same error)
- Blocked by a third-party service (Stripe, Neon, Vercel account issue)
- Completely lost conceptually (does not understand what they are trying to build)
- Frustrated to the point of disengagement (closing the laptop, giving up)
- Stuck for more than 60 minutes on the same issue with no new approaches

**Your response:** Step in. Unblock them. Environment issues are not learning opportunities -- they are friction that prevents learning. A student spending three hours fighting a Node.js version conflict is not learning anything about building software.

### The 60-Minute Rule

If a student has been stuck on the same problem for more than 60 minutes and has tried at least three different approaches (asking Claude, researching, starting fresh), intervene. Either:

1. Diagnose and fix the blocker yourself
2. Pair with them for 10 minutes to get past it
3. Tell them to skip it and move on (if it is not blocking the core deliverable)

The goal is forward momentum. Every day should end with something shipped, even if it is small.

---

## 7. Using Claude Code as a Teaching Tool

Claude Code is not just the tool students build with. It is also the best teaching tool in the curriculum. Here is how to use it.

### "Ask Claude to Explain"

When a student asks you a conceptual question -- "What is a webhook?" or "Why do we need environment variables?" -- do not answer it yourself. Say: "Ask Claude Code to explain that. Use plan mode."

Then follow up: "What did Claude say? Does that make sense? Can you explain it back to me?"

This teaches two skills at once: the concept itself, and the habit of using Claude Code as a research tool.

### Watch How They Prompt

A student's prompts reveal their understanding better than any quiz.

- **Vague prompts** ("make it look better") indicate they do not know what they want. Help them get specific: "What specifically is wrong with it? What would you change?"
- **Overly specific prompts** ("add a div with class flex items-center justify-between p-4") indicate they are trying to write code through Claude. Redirect: "Tell Claude what you want the user to see, not how to code it."
- **Good prompts** ("Add a notification bell in the header that shows unread count. When clicked, show a dropdown with the 5 most recent notifications. Mark them as read when the dropdown opens.") indicate they understand the product. Celebrate this.

### Plan Mode as a Teaching Moment

When a student is about to build something complex, have them start in plan mode:

> "Before you build this, ask Claude to plan it out. What approach would it take? What files would it change? What order?"

Review the plan together. Ask: "Does this plan make sense to you? Is anything missing? Would you change the order?" This develops architectural thinking without requiring the student to come up with the plan themselves.

### Debugging Together

When a student has an error and is stuck, do not fix it for them. Instead:

1. Have them copy the error message and paste it to Claude Code
2. Have them ask Claude what the error means and how to fix it
3. Watch what Claude suggests
4. Ask the student: "Does that fix make sense? What do you think it will change?"
5. Let them apply the fix and see the result

This loop -- error, explain, fix, verify -- is the core debugging workflow they will use for the rest of their career.

---

## 8. Phase Transition Notes

Each phase changes the nature of the work and your role as instructor. Here is what shifts and what to watch for.

### Phase 1 to Phase 2 (Week 3 to Week 4)

**What changes:** Students move from "learning the tools" to "understanding the stack." The work gets more conceptual. Phase 1 was about getting comfortable with Claude Code, Git, and deployment. Phase 2 is about understanding what Next.js, TypeScript, databases, and auth actually do.

**Your role shifts from:** Setup support and basic workflow coaching **to:** Conceptual guide. You are helping them understand *what* each tool does and *why* it exists, so they can give Claude Code clear direction.

**Watch for:** Students who got through Phase 1 on vibes -- they followed the steps but do not understand the workflow. Phase 2 will expose this because it requires them to describe what they want in domain-specific terms ("add a server component that fetches from the database" rather than "make a page that shows data").

**Intervention:** Have them re-read the Phase 1 guide.md files. Then ask them to explain the research-plan-execute workflow in their own words. If they cannot, spend 15 minutes walking through it with a concrete example.

### Phase 2 to Phase 3 (Week 6 to Week 7)

**What changes:** Students move from guided exercises to independent projects. Week 7 (portfolio) is the first project where they make all the decisions: what to include, how it looks, what the content says. This is a significant leap.

**Your role shifts from:** Conceptual guide **to:** Product coach. You are less concerned with whether they understand what a database is (they should by now) and more concerned with whether they can scope a project, make decisions, and ship.

**Watch for:** Decision paralysis. Some students freeze when the instructions stop being step-by-step. They spend three days choosing a color palette or agonizing over portfolio layout. Others rush and build something generic without doing the research phase.

**Intervention:** For the frozen: "Pick one option and go. You can change it later. The goal is to ship by Friday." For the rushers: "Show me your research. What portfolios did you study? What did you learn from them? Now plan before you build."

### Phase 3 Midpoint (Week 11 -- v1 Launch)

**What changes:** TeamTask Pro v1 ships. This is the biggest milestone in the curriculum. Students have built a complete multi-tenant SaaS with auth, payments, notifications, and dashboards. Weeks 12-14 add testing, monitoring, and CI/CD -- important but less exciting than building features.

**Watch for:** Post-launch fatigue. Students may feel like TeamTask Pro is "done" at Week 11 and lose motivation for the testing/monitoring/CI/CD weeks. Remind them: "You built a product. Now you are making it production-ready. These are different skills, and employers care about both."

**Intervention:** Show them a real-world example of a bug that testing would have caught, or a production incident that monitoring would have surfaced. Make the value concrete.

### Phase 3 to Phase 4 (Week 14 to Week 15)

**What changes:** Students go fully independent. Week 15 is "choose your own project." There is no guided project.md telling them what to build step by step. They decide everything.

**Your role shifts from:** Product coach **to:** Advisor. You are available for questions, but you should not be directing their work. If you find yourself telling a student what to build, you are doing too much.

**Watch for:** Students who cannot pick a project and waste days deciding. Students who pick something too ambitious (a social network, a marketplace with real-time chat) and cannot ship an MVP in a week. Students who pick something too simple (a static page with no backend) and do not challenge themselves.

**Intervention:** For scope: "Can you describe the core feature in one sentence? If it takes more than one sentence, it is too big for a two-week project." For ambition: "What is the simplest version of this that would be useful? Build that first."

### Phase 4 to Phase 5 (Week 18 to Week 19)

**What changes:** Building stops. Assembly begins. Students are no longer creating new things -- they are polishing, documenting, and presenting everything they have built.

**Your role shifts from:** Advisor **to:** Editor. You are reviewing their portfolio, their READMEs, their blog posts, their presentation. You are helping them tell their story clearly.

**Watch for:** Students who skip the writing and documentation. They want to build one more feature instead of writing a blog post about what they already built. Redirect them. The portfolio is the deliverable now, not more code.

**Intervention:** "Your portfolio is how the world sees your work. If your README has no screenshots, no one will click the demo link. If your blog post does not exist, no one knows what you learned. This week is about presentation, and it matters as much as the building."

---

## 9. Common Student Patterns

You will see the same patterns across every cohort. Here are the four most common and how to handle each.

### The Rusher

**Behavior:** Skips the research phase. Skips the planning phase. Goes straight to "Claude, build me a task management app." Accepts whatever Claude generates without reviewing it. Ships fast but ships broken. Quality gates fail and they ignore the failures.

**Why they do it:** They want to feel productive. Planning feels like not building. Research feels like procrastination. They measure progress by features, not quality.

**How to handle:**
- Make them show you their plan before they start executing. "What are you about to build? What is the data model? What does the user flow look like?"
- When their quality gates fail, do not let them ship. "Lint is failing. Type-check is failing. That means there are bugs. Fix them before you deploy."
- Reframe speed: "You are not fast if you have to redo it. You are fast if you do it right the first time. Planning is how you go fast."
- In ship review, ask them to demo a specific edge case they did not think about. When it breaks, the lesson lands.

### The Freezer

**Behavior:** Afraid to prompt Claude Code. Reads the guide three times. Takes extensive notes. Plans meticulously on paper. But never types a prompt. When they do prompt, they agonize over the wording for 20 minutes. Terrified of getting an error or breaking something.

**Why they do it:** Fear of failure. Fear of looking stupid. Perfectionism about inputs rather than outputs. Often has imposter syndrome -- "I am not technical enough to do this."

**How to handle:**
- Normalize errors. "Errors are information, not failure. Every professional developer sees errors all day. The skill is not avoiding errors -- it is responding to them."
- Give them a low-stakes first prompt: "Ask Claude to add a heading to the page. Just a heading. See what happens."
- Sit with them for their first few prompts. Your presence reduces the fear.
- Celebrate their first deployment, no matter how simple. "That is live on the internet. You did that."
- Remind them: "Claude Code does not judge your prompts. It just tries to help. There is no wrong way to ask."

### The Over-Researcher

**Behavior:** Spends three days in plan mode. Reads every resource link. Watches YouTube videos about the tools. Compares five different approaches. Has a beautiful Notion document outlining the architecture. Has not written a single prompt to build anything.

**Why they do it:** Research feels safe. Building feels risky. They want to fully understand before they start. In traditional education, understanding before doing was rewarded. Here it is a trap.

**How to handle:**
- Set a time limit on research: "You have until noon to research and plan. At noon, you start building. No exceptions."
- Reframe research as part of building: "The real learning happens when you direct Claude Code and see what it produces. Research is preparation. Building is the lesson."
- Ask them: "What question are you trying to answer with more research? If you started building right now, what is the worst thing that could happen?" Usually the answer is "I would have to redo something." That is fine. That is how iteration works.
- Point out that Claude Code itself is a research tool. They can ask Claude questions while building instead of front-loading all research.

### The Perfectionist

**Behavior:** Builds something good. Then rebuilds it. Then tweaks it. Then redesigns it. Pixel-perfect layouts. Refactors code they cannot even read. Misses ship deadlines because "it is not ready." Adds features that are not in the acceptance criteria because they "should" be there.

**Why they do it:** High standards, which is a strength -- until it prevents shipping. They confuse "production-ready" with "perfect." They fear being judged by their output.

**How to handle:**
- Give them the shipping mantra: "Done is better than perfect. Shipped is better than done. Live is better than shipped."
- Point to the acceptance criteria: "Does it meet every acceptance criterion? Yes? Then it ships. You can improve it later."
- Set hard deadlines: "This deploys by 4pm Friday. Whatever state it is in at 4pm, that is v1."
- Channel their energy: "You have high standards. Good. Apply them to the next project instead of endlessly polishing this one. Ship this, start fresh, and make the next one even better."
- Remind them that every professional product shipped imperfect. Version 1 of everything is embarrassing in hindsight. That is normal.

---

## 10. Week-by-Week Overview Table

Difficulty is rated 1-5 (1 = lightest, 5 = most demanding).

| Week | Phase | Title | Key Deliverable | Common Pitfalls | Difficulty |
|------|-------|-------|----------------|-----------------|------------|
| 1 | 1 | Your AI Development Environment | Personal landing page -- live URL | Environment setup issues, not deploying | 2 |
| 2 | 1 | The Claude Code Workflow | Multi-page site, clean Git history | Skipping research/plan, messy Git | 2 |
| 3 | 1 | Skills, Hooks, and Power Tools | Custom CLAUDE.md, skills, hooks | Over-engineering config, not testing hooks | 3 |
| 4 | 2 | The Modern Web Stack | Polished site with full frontend stack | Conceptual overload, not grasping server vs client components | 3 |
| 5 | 2 | Data, APIs, and Validation | Full-stack bookmark manager with database | Database setup confusion, env variable mistakes | 4 |
| 6 | 2 | Auth, Payments, and Services | Authenticated app with monitoring | Too many integrations at once, credential management | 4 |
| 7 | 3 | Portfolio + Blog | Portfolio site, optionally custom domain | Decision paralysis on design, skipping blog post | 3 |
| 8 | 3 | TeamTask Pro -- Foundation | SaaS with auth, orgs, CRUD | Data model complexity, multi-tenancy confusion | 4 |
| 9 | 3 | TeamTask Pro -- Payments | Stripe subscriptions + feature gating | Webhook handling, test mode confusion | 4 |
| 10 | 3 | TeamTask Pro -- Notifications | Email + in-app notifications | Notification architecture, email deliverability | 3 |
| 11 | 3 | TeamTask Pro -- Dashboard | Admin panel + charts, v1 launch | Scope creep before launch, dashboard overdesign | 3 |
| 12 | 3 | TeamTask Pro -- Testing | Vitest unit/integration + E2E tests | Testing feels abstract, low motivation post-launch | 4 |
| 13 | 3 | TeamTask Pro -- Monitoring | Sentry + PostHog instrumentation | "Why do I need this?", tracking plan confusion | 3 |
| 14 | 3 | TeamTask Pro -- CI/CD | Automated deploy pipeline, branch protection | GitHub Actions YAML debugging, secrets config | 4 |
| 15 | 4 | Independent Project (Week 1/2) | Deployed MVP of chosen project | Scope too big, stuck choosing, skipping plan | 4 |
| 16 | 4 | Independent Project (Week 2/2) | Production-ready independent project | Adding features instead of polishing, skipping tests | 4 |
| 17 | 4 | Advanced AI-Native Patterns | Open source PR or published npm package | Unfamiliar codebase overwhelm, PR anxiety | 5 |
| 18 | 4 | Speed Build | Complete app, idea to production in 5 days | Time pressure panic, cutting quality corners | 5 |
| 19 | 5 | Portfolio Assembly | Complete portfolio with all projects | Broken demo links, skipping documentation | 2 |
| 20 | 5 | The AI-Native Developer | Published retrospective, career-ready portfolio | Skipping the writing, not sharing publicly | 2 |

### Difficulty Curve Summary

Weeks 1-3 ramp gently. Weeks 4-6 introduce significant new concepts. Weeks 7-14 are the sustained build phase -- consistently demanding but within a familiar workflow. Weeks 15-18 are the peak: independent work, unfamiliar codebases, and time pressure. Weeks 19-20 are deliberately lighter to allow reflection and polish.

The hardest single week is usually Week 18 (Speed Build) because it combines time pressure with full independence. The most conceptually dense week is usually Week 5 (databases, APIs, and validation) because it introduces more new ideas than any other week.

---

## Final Notes

This curriculum works because it is built on a simple loop: research, plan, execute, verify, ship. Your job as instructor is to keep students in that loop. When they skip steps, bring them back. When they get stuck, unblock them. When they ship, celebrate it.

The students who succeed are not the ones who understand the most code. They are the ones who ship the most consistently. Protect the shipping rhythm above all else.

You have a well-structured curriculum, clear weekly deliverables, and an AI agent that does the building. Trust the structure. Facilitate, do not lecture. Keep them shipping.

Good luck. Now go teach.
