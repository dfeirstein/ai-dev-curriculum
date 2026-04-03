# Week 4 Guide: The Modern Web Stack

This week is about building your vocabulary. Not code vocabulary -- architectural vocabulary. Every tool below is something you'll direct Claude Code to use. Your job is to understand what each one does, why it matters, and what to say when you want Claude Code to use it well.

You will never need to write code with any of these tools. You need to know what they are so you can make good decisions and give clear direction.

---

## Part 1: Next.js -- Your Application Framework

### What It Is

Next.js is a framework that adds structure, routing, and server-side capabilities to React. If your website were a building, Next.js is the architectural blueprint -- it determines how rooms connect, where the doors go, and how people move through the space.

You've been using Next.js since Week 1. Now you'll understand the pieces that make it powerful.

### App Router: Pages Are Just Folders

Next.js uses a file-based routing system called the App Router. The concept is simple: the folder structure of your project IS your site's URL structure.

- `app/page.tsx` is your homepage (`/`)
- `app/about/page.tsx` is your about page (`/about`)
- `app/projects/page.tsx` is your projects page (`/projects`)
- `app/blog/[slug]/page.tsx` is a dynamic page -- one template, many URLs (`/blog/my-first-post`, `/blog/another-post`)

You don't configure a routing table or wire up URL mappings. You create folders. That's it.

When directing Claude Code: "Add a new page at `/contact` with a heading and a contact form."

### Layouts: Define Once, Used Everywhere

A layout is shared UI that wraps multiple pages. Your navigation bar, footer, and overall page structure go in a layout. Every page inside that layout automatically gets wrapped with it.

This means you define your nav and footer once, and every page inherits them. Change the nav in one place, it updates everywhere.

When directing Claude Code: "Create a root layout with a navigation bar and footer that wraps all pages."

### Server Components vs Client Components

This is an important concept for giving Claude Code architectural direction.

**Server components** run on the server before the page reaches the browser. They're fast, they can talk directly to databases, and they don't add JavaScript to what the user downloads. Most of your pages should be server components.

**Client components** run in the browser. They handle interactive things -- clicking buttons, opening menus, toggling dark mode, filling out forms. Anything that responds to user actions needs to be a client component.

The rule of thumb: **server by default, client only when you need interactivity.** This is a performance decision. Server components make your site faster because less JavaScript gets sent to the browser.

When directing Claude Code: "Use Next.js App Router. Server components by default. Only use client components for interactive elements like the mobile menu toggle and dark mode switch."

### Loading and Error States

Next.js has built-in patterns for handling two common situations:

- **Loading states:** What shows while data is being fetched? A spinner? A skeleton? A blank page? Next.js lets you define this so users never stare at nothing.
- **Error states:** What shows when something breaks? Instead of a cryptic crash, you show a friendly message.

When directing Claude Code: "Add loading states for any pages that fetch data. Add an error boundary that shows a friendly message if something breaks."

---

## Part 2: TypeScript -- Your Safety Net

### What It Is

TypeScript is not a language you need to learn. It's a quality gate.

Here's what it does: it catches entire categories of bugs before your app ever runs. Think of it like spell-check for code. Spell-check doesn't help you write a better essay -- but it catches obvious mistakes that would make you look sloppy.

When Claude Code writes TypeScript and it passes the type checker, you know the code is structurally sound. Functions receive the right inputs. Data has the right shape. Pages get the right props. Things connect the way they're supposed to.

### Why It Matters for AI-Native Development

This is where TypeScript becomes especially valuable for you. When an AI writes code, you need automated ways to verify quality. You can't read every line. But you can run a type checker.

- `npm run typecheck` passes? The code structure is valid.
- `npm run typecheck` fails? Something is structurally wrong, and the error message tells Claude Code exactly what to fix.

TypeScript turns structural correctness into a yes/no question. That's incredibly powerful when you're directing an AI.

### Strict Mode

TypeScript has a "strict mode" setting that enables the highest level of checking. You always want this on. It catches more bugs, and it forces Claude Code to write more precise code.

When directing Claude Code: "TypeScript strict mode, always."

### Your Relationship with TypeScript

You don't need to read type annotations. You don't need to understand generics or interfaces. You need to know one thing: **did `npm run typecheck` pass?** If yes, the code structure is valid. If no, paste the error back to Claude Code and tell it to fix it.

That's your entire TypeScript workflow.

---

## Part 3: Tailwind CSS -- Your Design Language

### What It Is

Tailwind CSS is a system for styling your app using utility classes instead of writing custom design rules from scratch. Instead of creating a custom style called "hero-heading" and defining its font size, weight, and color separately, Tailwind lets you compose the style from a library of small, descriptive building blocks.

You don't need to know the class names. Claude Code knows them. What you need to understand is the mental model.

### Why It Works Great with AI

Tailwind is descriptive and composable. Each utility class does one thing and has a clear name. This makes it precise -- when you tell Claude Code "make the heading larger with more spacing below it," Tailwind gives Claude Code exact tools to do that. There's no ambiguity, no inventing custom names, no wondering if two styles conflict.

The result: consistent, predictable styling that Claude Code can apply accurately.

### Responsive Design

Tailwind has built-in breakpoints for different screen sizes. Your app can automatically look different on a phone, tablet, and desktop -- all from the same code.

The approach is "mobile-first." You design for the smallest screen first, then add adjustments for larger screens. This is a direction you give Claude Code, not something you implement yourself.

When directing Claude Code: "Style with Tailwind CSS. Mobile-first responsive design. Use consistent spacing and a clean visual hierarchy."

### The Design Conversation

When you want to change how something looks, you describe the visual outcome you want:

> "Make the card shadows more subtle."

> "Increase the spacing between sections."

> "The text should be larger on desktop but stay the same on mobile."

> "Use a consistent color palette -- primary blue, neutral grays, white backgrounds."

Claude Code translates your visual direction into Tailwind classes. You review the result in the browser.

---

## Part 4: shadcn/ui -- Your Component Library

### What It Is

shadcn/ui is a collection of pre-built, professionally designed UI components. Buttons, forms, cards, dialogs, dropdown menus, navigation bars, command palettes -- the building blocks of modern web applications.

### Why "Own" Matters

Here's what makes shadcn/ui different from other component libraries: when you install a component, the actual code gets copied into your project. You own it completely. You can customize anything.

Other libraries give you a black box -- you use their button and hope it looks the way you want. With shadcn/ui, the button code is right there in your project. Claude Code can modify it however you need.

This is a big deal for AI-native development. Claude Code can read, understand, and modify your components because they're part of your codebase, not hidden inside a library.

### How You'll Use It

Instead of telling Claude Code to build a button from scratch, you tell it to use the shadcn Button component. Instead of describing exactly how a dialog should work, you tell it to use the shadcn Dialog component.

This gives you professional-quality UI with minimal direction. The components handle accessibility, keyboard navigation, animations, and styling out of the box.

When directing Claude Code: "Use shadcn/ui components. Install whatever's needed. Use the Button, Card, and NavigationMenu components for the site."

### What's Available

Browse the shadcn/ui component gallery (link in resources.md) to see what's available. When you see a component you want, you just tell Claude Code to use it. Some highlights:

- **Button:** Every variation you'd need -- primary, secondary, outline, ghost
- **Card:** Content containers with headers, descriptions, and footers
- **Dialog:** Modal windows that overlay the page
- **NavigationMenu:** Site navigation with dropdowns
- **Sheet:** Slide-in panels (great for mobile menus)
- **Command:** A command palette (the spotlight-search-like interface)
- **Form:** Full form handling with validation

---

## Part 5: ESLint + Prettier -- Your Code Quality Cops

### ESLint: The Pattern Checker

ESLint scans code for bad patterns and potential bugs. It's like a building inspector who checks for code violations -- things that technically work but are likely to cause problems.

When Claude Code writes code, ESLint checks it. If ESLint complains, there's usually a real issue worth fixing.

Run it: `npm run lint`

### Prettier: The Formatter

Prettier makes code formatting consistent. Indentation, line breaks, quote styles -- all standardized automatically. This doesn't affect how your app works or looks. It keeps the codebase tidy, which makes Claude Code more effective (clean code is easier for AI to read and modify).

### Your Workflow

These are "set up once, trust forever" tools. You direct Claude Code to configure them at the start of a project. After that, they run as quality gates:

1. Claude Code makes changes.
2. You run `npm run lint` -- any pattern issues?
3. You run `npm run typecheck` -- any structural issues?
4. You run `npm run build` -- does everything compile?

Three commands. If all three pass, the code is mechanically sound. Then you check it visually in the browser. That's your full quality pipeline.

When directing Claude Code: "Set up ESLint and Prettier with the recommended configs. Add lint and typecheck scripts to package.json."

---

## Part 6: Putting It All Together

### The Full Stack Mental Model

Here's how all these pieces fit together:

- **Next.js** is the framework -- it provides structure, routing, and the server/client split.
- **TypeScript** is the safety net -- it catches structural bugs automatically.
- **Tailwind CSS** is the design language -- it makes styling precise and consistent.
- **shadcn/ui** is the component library -- it gives you professional building blocks.
- **ESLint + Prettier** are the quality cops -- they catch bad patterns and enforce consistency.

When you start a new project, your opening prompt to Claude Code should include all of these:

> "Create a Next.js project with App Router, TypeScript strict mode, Tailwind CSS, and shadcn/ui. Set up ESLint and Prettier. Server components by default, client components only for interactivity."

That one prompt sets up a professional-grade codebase. Everything after that is giving creative and architectural direction.

### The Quality Pipeline

After every significant change Claude Code makes, run:

1. `npm run lint` -- code quality check
2. `npm run typecheck` -- structural integrity check
3. `npm run build` -- full compilation check
4. Visual inspection in the browser -- does it look right?
5. Interaction check -- does it work right?

If all five pass, you're good to ship.

---

## What's Next

You've got the vocabulary. Now use it. Head to [project.md](project.md) to rebuild your personal site with the full modern stack.
