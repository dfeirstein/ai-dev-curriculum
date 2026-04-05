# Week 3: Skills, Hooks, and Power Tools — Instructor Guide

## Week at a Glance
- Theme and key concepts: Customizing Claude Code with skills, hooks, and MCP servers. Students learn the difference between reusable instructions (skills), automated triggers (hooks), and external tool integrations (MCP). They prove it works by adding a new page using their custom workflow.
- What students ship: An enhanced personal website with a comprehensive CLAUDE.md, at least one custom skill, at least one hook, and a new page built using the custom setup
- Estimated hours: 6-8
- Difficulty: 3

## Pacing Guide
- **Day 1-2:** Read guide.md. Upgrade CLAUDE.md to be comprehensive. Install and configure skills. Understand the skill format and when to use them.
- **Day 3-4:** Set up hooks (pre-commit lint check, build verification). Configure MCP servers. Set up Chrome tool if applicable. Test everything works together.
- **Day 5:** Prove the setup works: add a new page to the site using the full custom workflow. Every hook fires, every skill is available. Deploy.

## Getting Students Started
- Kick off by demonstrating the problem: show a Claude Code session without any customization — it guesses at conventions, misses quality checks, produces inconsistent output. Then show the same task with a proper CLAUDE.md, skills, and hooks. The contrast sells the investment.
- Common confusion: students conflate skills, hooks, and CLAUDE.md. Clarify the mental model early. CLAUDE.md = project context that is always loaded. Skills = reusable prompt templates Claude can invoke. Hooks = automated actions that fire on events (like pre-commit).
- Students should have open: Ghostty terminal, their Week 2 project, and the Claude Code documentation for skills and hooks.

## Check-in Points
- **After guide.md:** Can the student explain when to use a skill vs a hook vs CLAUDE.md? A simple test: "Where would you put a rule about always using Tailwind?" (CLAUDE.md). "Where would you put a reusable prompt for creating a new page component?" (Skill). "Where would you put an automatic lint check before every commit?" (Hook).
- **Mid-project checkpoint:** CLAUDE.md is comprehensive and project-specific. At least one skill is installed and the student can demonstrate invoking it. At least one hook is configured. If they only have the CLAUDE.md done, they are behind — hooks and skills should be in progress.
- **Pre-ship review:** Run through the full cycle: student creates a branch, uses a skill to scaffold something, commits (hook fires automatically), and the output meets the quality standards in CLAUDE.md. If any piece is broken or skipped, fix it before shipping.

## Common Pitfalls
- **Over-engineering skills.** A student creates 15 hyper-specific skills for every possible scenario. They don't need a skill for "add a paragraph to the About page." Skills should be for repeated patterns — creating a new page component, setting up a form, adding a new section with consistent structure.
- **Not understanding hooks vs skills.** Student puts automated checks in a skill (which requires manual invocation) instead of a hook (which fires automatically). If a student says "I have a skill that runs lint," redirect them — that should be a hook.
- **Chrome tool / MCP setup failures.** The Chrome extension or MCP server configuration can be finicky. If a student is stuck on MCP setup for more than 30 minutes, have them skip it and come back. Don't let tooling setup block the core learning about skills and hooks.
- **CLAUDE.md is a copy-paste from a template.** If their CLAUDE.md reads like generic advice that could apply to any project, it is not useful. Push for specifics: actual file paths, actual component patterns, actual naming conventions from their project.
- **Forgetting to test hooks.** Student adds a pre-commit hook but never actually triggers it. Have them make a deliberate mistake (break a lint rule) and verify the hook catches it.

## How to Know the Student Gets It
- They can explain the difference between skills, hooks, and CLAUDE.md without looking at notes, and give a correct example of when to use each.
- When starting a new task, they instinctively check if they have a skill for it or if they should create one, rather than writing a one-off prompt from scratch.
- Their hooks actually fire and catch real issues — they can show you a commit that was blocked by a hook and explain what they fixed.
- Red flags: All customization is in CLAUDE.md with no skills or hooks. Student installed skills but never actually uses them. Hooks are configured but broken (silently passing everything). Student cannot explain what MCP stands for or what it does.

## Support Strategies
- If stuck on skills vs hooks: Use a physical analogy. A skill is like a recipe card you pull out when you want to cook something. A hook is like a smoke detector — it is always monitoring and fires automatically when something goes wrong.
- If rushing without reviewing: Ask them to break something on purpose and show you the hook catching it. Ask them to invoke a skill and walk you through what it does. This forces engagement with the tools.
- If frustrated: Usually frustrated with configuration files and YAML/JSON syntax. Remind them to have Claude Code write all configuration. Their job is to describe what they want the hook or skill to do, not to write the config by hand.
- `/powerup` is especially useful this week: "Automate your workflow," "Extend with tools," "Dial the model," and "Plan mode & context management" all map directly to this week's content. Assign them as pre-work or as reinforcement for students who need extra practice.

## Differentiation
- **Moving fast?** Create a multi-step skill that scaffolds an entire new page with component, tests, and routing. Set up the Chrome MCP tool and demonstrate using it for visual QA. Add a post-deploy hook that verifies the live site loads.
- **Struggling?** Focus on CLAUDE.md and one hook (pre-commit lint). Skip custom skills and MCP for now — they can add those in later weeks as they get more comfortable.
- **Minimum viable ship:** A comprehensive CLAUDE.md, one working hook that fires on commit, and evidence they used their setup to build at least one new thing. The new page or section is deployed live.
