# Project: Rebuild Your Personal Site with the Full Stack

**What you're building:** A polished, architecturally sound personal website using Next.js App Router, TypeScript strict mode, Tailwind CSS, and shadcn/ui components. Dark mode, responsive design, deployed to Vercel.

**Time estimate:** 6-8 hours

**What you'll need open:** Ghostty (your terminal) and a web browser

---

## The Brief

Your Week 2 site works, but it was built before you understood the full stack. Now you know what these tools are and why they matter. Time to rebuild with proper architecture.

This isn't about changing what your site looks like (though it will look better). It's about directing Claude Code to build it the right way -- with the right tools, the right patterns, and the right quality gates. The result will be faster, more maintainable, and ready to grow.

---

## Phase 1: Research

Start Claude Code and begin with research. You're not building yet -- you're getting informed.

> "Claude, what are the best practices for structuring a personal website with Next.js App Router, TypeScript, Tailwind CSS, and shadcn/ui? I want a multi-page site with Home, About, Projects, and Contact pages. What should the architecture look like?"

Read what Claude Code tells you. Ask follow-up questions:

> "What shadcn/ui components would make sense for a personal site?"

> "How should I handle dark mode with shadcn/ui and Tailwind?"

> "What's the best way to organize shared components like the nav and footer?"

Take note of the key decisions. You're gathering information to make your plan.

---

## Phase 2: Plan

Now ask Claude Code to create a plan:

> "Claude, plan a rebuild of my personal site using Next.js App Router, TypeScript strict mode, Tailwind CSS, and shadcn/ui components. Keep the same pages (Home, About, Projects, Contact) but improve the architecture. Include: a root layout with nav and footer, dark mode toggle, responsive design, and shadcn/ui components. Give me the plan step by step before building anything."

Review the plan. Does it cover everything? Ask questions if anything is unclear. Approve it before moving on.

---

## Phase 3: Execute

### Step 1: Create the New Project

> "Claude, create a new Next.js project called my-site-v2 with TypeScript strict mode, Tailwind CSS, App Router, ESLint, and Prettier. Set up the recommended configurations."

Wait for the project to be created. Then:

> "Claude, initialize shadcn/ui and configure it for this project."

### Step 2: Set Up the Foundation

> "Claude, create a root layout with a responsive navigation bar using shadcn/ui NavigationMenu and a footer. The nav should have links to Home, About, Projects, and Contact. On mobile, use a Sheet component for the menu."

Check the browser. Does the nav show up? Does the mobile menu work? Give feedback if needed.

### Step 3: Build the Homepage

> "Claude, build the homepage with: a hero section with my name and tagline, a brief about section with a shadcn/ui Card, and a featured projects preview using a grid of Cards. Use clean typography and generous spacing. Server component."

Review in the browser. Iterate:

> "The spacing between sections needs to be larger."

> "Make the hero section more visually striking."

### Step 4: Build the About Page

> "Claude, build the About page with a longer bio, a skills or interests section using shadcn/ui Badge components, and a timeline or highlights section. Keep the same design language as the homepage."

### Step 5: Build the Projects Page

> "Claude, build the Projects page with a grid of shadcn/ui Cards. Each card should have a title, description, tech tags using Badge, and a link to the project. Use placeholder content for now -- I'll fill in real projects later."

### Step 6: Build the Contact Page

> "Claude, build the Contact page with a contact form using shadcn/ui Form components. Fields: name, email, message. The form doesn't need to actually send anywhere yet -- just the UI. Add my social links below the form."

### Step 7: Add Dark Mode

> "Claude, add a dark mode toggle using shadcn/ui and next-themes. Put the toggle in the nav bar. It should switch between light, dark, and system preference. Remember the user's choice."

Test the toggle. Switch between modes. Check every page in both themes.

### Step 8: Polish Responsive Design

> "Claude, review all pages for mobile responsiveness. Make sure everything looks great on phone, tablet, and desktop. The nav should collapse to a mobile menu on small screens. Cards should stack on mobile and grid on desktop."

Test by resizing your browser window. Check on your phone if possible.

### Step 9: Run Quality Gates

Time to verify everything mechanically:

> "Claude, run npm run lint and fix any issues."

> "Claude, run npm run typecheck and fix any issues."

> "Claude, run npm run build and make sure it completes without errors."

All three must pass before you ship.

### Step 10: Deploy

> "Claude, initialize a git repo, commit everything with the message 'Rebuild personal site with full modern stack', create a GitHub repo called my-site-v2, push, and deploy to Vercel."

Visit your live URL. Check everything:

- Does the site load?
- Does navigation work on all pages?
- Does dark mode toggle work?
- Does it look good on mobile?
- Do all links point to the right places?

---

## Acceptance Criteria

Your project is complete when all of these are true:

- [ ] Site is live on Vercel at a public URL
- [ ] Built with Next.js App Router, TypeScript strict mode, Tailwind CSS, and shadcn/ui
- [ ] Has pages: Home, About, Projects, Contact
- [ ] Has a responsive navigation bar that collapses on mobile
- [ ] Has a working dark mode toggle that remembers preference
- [ ] All pages are responsive -- look good on phone, tablet, and desktop
- [ ] Uses shadcn/ui components (Cards, Buttons, Badges, NavigationMenu, Sheet, Form)
- [ ] `npm run lint` passes
- [ ] `npm run typecheck` passes
- [ ] `npm run build` completes without errors
- [ ] Code is on GitHub in a public repository

---

## Stretch Goals

**Add Animations with Framer Motion**
> "Claude, add subtle entrance animations to the page sections using Framer Motion. Cards should fade in and slide up when they enter the viewport. Keep it smooth and not distracting."

**Add a Command Palette**
> "Claude, add a command palette using shadcn/ui's Command component. Pressing Cmd+K should open it. Include quick navigation to all pages and a toggle for dark mode."

**Add an RSS Feed**
> "Claude, add an RSS feed for the blog page so people can subscribe to updates."

**Add SEO Metadata**
> "Claude, add proper SEO metadata to every page -- title, description, Open Graph tags. When someone shares a link to my site on social media, it should show a nice preview."

---

## Troubleshooting

**shadcn/ui components aren't styling correctly**
> "Claude, the shadcn/ui components look unstyled. Check that the Tailwind configuration includes the shadcn paths and that the CSS variables are set up correctly."

**Dark mode isn't working**
> "Claude, the dark mode toggle doesn't change anything. Check the next-themes setup and make sure the ThemeProvider is wrapping the app in the root layout."

**Type errors after adding components**
> "Claude, I'm getting TypeScript errors after adding shadcn/ui components. Run typecheck and fix all the issues."

**Mobile menu won't close after navigation**
> "Claude, when I click a link in the mobile menu, it navigates but the menu stays open. Make it close after navigation."

---

## Reflection

Compare your Week 2 site with this rebuild. Same content, vastly different quality. The difference isn't that you learned to code -- it's that you learned to give better direction. You understood what tools were available, what they're for, and how to tell Claude Code to use them properly.

That architectural vocabulary is what separates someone who can make a thing from someone who can make a thing well.
