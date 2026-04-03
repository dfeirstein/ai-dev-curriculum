# Week 15 Guide: Choosing and Building Your Own Project

---

## Part 1: How to Choose What to Build

### The Best Ideas Come from Experience

The strongest project ideas solve a problem you've personally experienced. Not a hypothetical problem. Not something you think other people might need. A problem you've actually felt.

Why? Because when you've felt the pain, you understand the problem deeply. You know what a solution should look like. You can tell immediately whether what you're building is actually useful -- not because you ran a survey, but because you'd use it yourself.

Ask yourself:

- What task do I do repeatedly that feels harder than it should be?
- What app do I use that frustrates me? What would I change about it?
- What spreadsheet or manual process could be replaced by a simple tool?
- What would I build if I knew it would only take a week?

That last question is important. Because it will only take a week. You've proven that.

### The Trap: Building Something Too Big

The most common mistake in independent projects is choosing something too ambitious. You get excited about a massive vision and try to build all of it at once. Then you run out of time, nothing is finished, and you have a half-built product that doesn't work.

The antidote is the MVP -- the Minimum Viable Product. This isn't a buzzword. It's a survival strategy.

### What Is an MVP?

An MVP is the smallest version of your idea that delivers value to someone. Not the smallest version that looks impressive. Not the smallest version that has all the features you want. The smallest version that actually works and solves the core problem.

Here's how to find it:

1. **Write down every feature you can imagine.** Don't filter. Just list them all.
2. **Circle the one thing that matters most.** The core action. The reason anyone would use this at all.
3. **Ask: can someone get value from just that one thing?** If yes, that's your MVP.
4. **Add back features only if the core doesn't work without them.** Authentication is usually necessary. A notification system probably isn't -- not for the MVP.

Example: You want to build a recipe manager. The full vision includes meal planning, shopping lists, nutritional info, social sharing, and photo uploads. The MVP? Save recipes and find them later. That's it. If that works and is useful, everything else can come later.

### Choosing What Fits Your Stack

You know Next.js, TypeScript, Postgres, Drizzle, Zod, Better Auth, Stripe, Tailwind, shadcn/ui, Vercel, Sentry, PostHog, GitHub Actions. That's a powerful stack. But not every project needs every piece.

Ask yourself for each tool:

- **Next.js + TypeScript + Tailwind + Vercel:** Yes, always. This is your foundation.
- **Postgres + Drizzle:** Does your project store data beyond what a single user has in their browser? If yes, you need a database.
- **Better Auth:** Do users need accounts? Will they come back and see their data? If yes, you need auth.
- **Stripe:** Does your project involve payments? If not, skip it for the MVP.
- **External APIs:** Does your project need data from somewhere else? Weather, AI, maps, social media?
- **Real-time features:** Does anything need to update live without refreshing? Usually not for an MVP.

Be honest about what you need versus what would be cool. Cool features that aren't essential are Week 16 work, not Week 15 work.

---

## Part 2: Research Phase

### Competitive Analysis

Before you build anything, find out what already exists. This isn't discouraging -- it's empowering. Knowing what's out there helps you:

- **Avoid reinventing something that already works well.** If a perfect solution exists, pick a different problem.
- **Find gaps.** Every existing tool has things it does poorly. That's your opportunity.
- **Steal good ideas.** Not code -- ideas. UI patterns that work. Feature decisions that make sense. Onboarding flows that feel smooth.
- **Avoid bad ideas.** If every competitor made the same mistake, you can learn from it.

How to research:

1. Search for your idea. "Best [your idea] app" or "[your idea] tool."
2. Try 3-5 existing tools. Actually sign up and use them.
3. Take notes: What do they do well? What's frustrating? What's missing?
4. Read reviews -- app store reviews, Reddit threads, Twitter complaints. Users tell you exactly what's wrong.

### Feature Mapping

After researching, create three lists:

**Must-Have (MVP):**
Features the product literally cannot work without. If you cut these, there's no product.

**Nice-to-Have (Week 16):**
Features that make it significantly better but aren't required for the core to work.

**Later (Post-Curriculum):**
Features that would be great eventually but are clearly beyond the two-week scope.

Be ruthless about what goes in the Must-Have list. Every feature you add to the MVP is a feature that might prevent you from finishing.

### User Flows

Before you touch Claude Code, sketch out the user flows. Not in a design tool -- just in your head or on paper.

- What does a new user see first?
- What's the main action they take?
- What happens after that action?
- Where do they go next?

This is your roadmap. When you sit down with Claude Code, you'll describe these flows, not individual features.

---

## Part 3: Planning Phase

### Architecture Decisions

This is the research-then-plan phase that you've practiced throughout the curriculum. For an independent project, it's even more critical because there's no guide telling you what to build next.

Direct Claude Code to help you plan:

> "I'm building a [description of your project]. It needs [list of must-have features]. I want to use Next.js, TypeScript, Postgres with Drizzle, and Better Auth. Help me plan the architecture: what database tables do I need, what pages should exist, and what's the best way to organize the project?"

Claude Code is an excellent planning partner. It will think through your data model, suggest page structures, and flag things you might have missed. But you make the final decisions. Review its plan, ask questions, push back on anything that seems overly complex.

### Data Model First

The data model is the foundation of everything. Get this right and building goes smoothly. Get it wrong and you'll be restructuring mid-build.

For your planning session, ask Claude Code:

> "Based on this feature set, design the database schema. Show me the tables, their columns, and how they relate to each other. Keep it as simple as possible for an MVP."

Review what it suggests. Ask yourself:

- Does every table serve a must-have feature?
- Are the relationships logical?
- Is anything overly complex for what I actually need?

### Page Structure

Map out every page your MVP needs:

- Landing/home page
- Auth pages (sign in, sign up) -- if you need auth
- The main application page(s)
- Any settings or profile pages

Fewer pages is better for an MVP. Every page is another thing to build, test, and maintain.

---

## Part 4: The Build Sprint

### Days 4-5: Build Mode

Once your plan is solid, building is the part you know best. You've done this for 14 weeks. The workflow doesn't change:

1. **Start with the foundation.** Project setup, database, auth. The boring stuff that everything else depends on.
2. **Build one feature at a time.** Don't try to build everything in parallel. Finish one thing, verify it works, move to the next.
3. **Test as you go.** After each feature, check it in the browser. Does it work? Does it look right? Fix issues before moving on.
4. **Commit frequently.** Every working feature gets a commit. Small, frequent saves mean you can always go back.
5. **Deploy early.** Get it on Vercel as soon as there's anything to see. Deploying early catches environment issues before they become deadline issues.

### When You Get Stuck

You will get stuck. This is normal, especially on an independent project where you're making all the decisions. When it happens:

- **Describe the problem to Claude Code.** "This feature isn't working as expected. Here's what I see: [description]. Here's what I expected: [description]. Help me figure out what's wrong."
- **Simplify.** If a feature is taking too long, ask yourself: is there a simpler version that still works?
- **Move on temporarily.** If one feature is blocking you, build something else and come back to it.
- **Don't spiral.** Spending three hours on a nice-to-have feature while your core functionality isn't done is a bad trade. Prioritize ruthlessly.

### The MVP Gut Check

At the end of Week 15, your project should pass this test:

- Can someone visit the URL and understand what it does?
- Can they perform the core action?
- Does it work without crashing?

If yes, you have an MVP. Everything else is Week 16 work.

---

## Part 5: Applying Your Full Stack Knowledge

You've learned a lot in 14 weeks. Here's a quick reference for which patterns from the curriculum apply to common project types:

**If your project has user accounts:**
- Better Auth for authentication (Week 8)
- Protected routes and session management
- User-specific data queries

**If your project has payments:**
- Stripe integration (Week 9)
- Webhook handling for payment events
- Subscription or one-time payment flows

**If your project has a dashboard:**
- Data aggregation and visualization (Week 11)
- Charts and metrics display

**If your project has real-time features:**
- Server-Sent Events or polling
- Optimistic UI updates

**If your project uses external APIs:**
- Server-side API calls (keep keys secure)
- Error handling for third-party failures
- Caching responses when appropriate

You don't need to remember how to implement any of this. You need to know it exists so you can direct Claude Code to build it. "Add Stripe checkout for a one-time $10 payment" is all you need to say -- you've seen how it works.
