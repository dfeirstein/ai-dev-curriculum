# Project: Personal Portfolio + Blog

**What you're building:** A personal portfolio site with project showcases, about page, blog with MDX posts, SEO optimization, and polished design. Your public identity as a builder.

**Time estimate:** 10-12 hours

**What you'll need open:** Ghostty (your terminal), a web browser for research, and your notes on what to include.

---

## The Brief

This is your site. It represents you. It's the link you share when someone asks what you're building. Take it seriously -- not by making it perfect, but by making it genuine. A simple, clean portfolio with real content beats an over-designed one with placeholder text every time.

---

## Phase 1: Research

### Study Existing Portfolios

Spend 30-60 minutes looking at portfolios. Take notes on what you like and why. Here are some things to pay attention to:

- How many pages do they have?
- What's the first thing you see?
- How do they showcase projects?
- What does the blog look like?
- What's the overall vibe -- minimal, playful, corporate, creative?

Ask Claude Code for input:

> "Claude, I'm planning a personal portfolio and blog. What are the essential pages and sections? What patterns work well for showcasing projects when I only have a few so far?"

> "Claude, what's the best way to structure a Next.js portfolio with an MDX blog? Should I use a content layer library or handle MDX files directly?"

### Define Your Content

Before writing any code, decide:

1. **Your one-liner.** How do you describe yourself in one sentence? "Builder learning to create software with AI" is a perfectly good start.
2. **Your projects.** List every project you can showcase (bookmark manager at minimum). For each, write a one-paragraph description.
3. **Your first blog post topic.** What's something you've learned that others would find interesting?
4. **Your design preference.** Minimal and clean? Bold and colorful? Dark mode? Light mode? Having a direction helps Claude Code make better design choices.

---

## Phase 2: Plan

Start Claude Code in a new project directory and create a plan:

> "Claude, I want to build a personal portfolio and blog. Here's what I want:
>
> **Pages:**
> - Home: hero section with my name, one-liner, and links to projects and blog
> - Projects: showcase of my work with descriptions, tech stack, and links
> - Blog: MDX-powered blog with individual post pages
> - About: who I am, my background, what I'm building toward
> - Contact: ways to reach me
>
> **Technical requirements:**
> - Next.js with TypeScript, Tailwind, and shadcn/ui
> - MDX for blog posts stored as files in the repo
> - SEO metadata on every page (title, description, Open Graph)
> - Sitemap and robots.txt
> - Responsive design (looks good on phone and desktop)
> - Dark mode support
> - Fast -- minimal JavaScript, optimized images
>
> **Design direction:** [describe your preference -- e.g., 'clean and minimal with lots of whitespace' or 'dark theme with accent colors']
>
> Plan the full build: project structure, pages, components, content setup, and deployment."

Review the plan. Ask yourself:

- Does the page structure match what I want?
- Is the blog setup straightforward? (MDX files in a content directory, rendered as pages)
- Are SEO and OG tags included for every page?
- Will the design match my vision?

---

## Phase 3: Execute

### Step 1: Project Setup

> "Claude, create the Next.js project with TypeScript, Tailwind, and shadcn/ui. Initialize the project structure with the pages we planned. Set up the CLAUDE.md file with project conventions."

Run the quality gates immediately:

> "Claude, run npm run lint, npm run typecheck, and npm run build. Fix any issues."

### Step 2: Layout and Navigation

> "Claude, build the site layout: a navigation bar with links to Home, Projects, Blog, About, and Contact. Add a footer with social links. Make the nav responsive -- hamburger menu on mobile. Add dark mode toggle."

Test: resize your browser. Does the nav collapse on mobile? Does the dark mode toggle work?

### Step 3: Home Page

> "Claude, build the home page. Hero section with my name, '[your one-liner]', and call-to-action buttons linking to Projects and Blog. Below that, a brief section showing 2-3 featured projects and recent blog posts. Keep it clean and focused."

### Step 4: Projects Page

> "Claude, build the projects page. Create a project card component that shows: project name, description, tech stack tags, a screenshot placeholder, and links to live demo and GitHub. Here are my projects: [list your projects with descriptions]. Make the layout a responsive grid."

### Step 5: Blog Setup with MDX

> "Claude, set up MDX for the blog. Blog posts should be .mdx files in /content/blog with frontmatter for title, date, description, and tags. Create /blog as an index page listing all posts (title, date, description, tags) sorted by newest first. Create /blog/[slug] for individual posts with proper typography and reading experience. Add estimated reading time."

### Step 6: Write Your First Blog Post

This step is on you, not Claude Code. Write a real blog post. Create a `.mdx` file in your content directory.

Some ideas for your first post:
- "What I learned building software with AI in 6 weeks"
- "From zero to full-stack: my experience as an AI-native builder"
- "Why I'm learning to build software (and why I'm not learning to code)"

You can ask Claude Code for help with structure:

> "Claude, I want to write a blog post about [topic]. Help me outline it -- suggest a structure with sections and key points to cover. Don't write it for me, just give me the skeleton."

Then write the post yourself. Your voice is what makes it interesting.

### Step 7: About Page

> "Claude, build the about page. Include a section for my background, what I'm currently building, and what I'm interested in. Here's my content: [paste your about text]. Make it warm and readable, not a resume."

### Step 8: Contact Section

> "Claude, add a contact section. Include links to my GitHub, [any other platforms you use], and an email address. You can make this a section on the about page or a separate page -- whichever fits the design better."

### Step 9: SEO and Metadata

> "Claude, add comprehensive SEO to every page. Each page needs: a unique title tag, meta description, Open Graph title/description/image, and Twitter card metadata. Generate a sitemap.xml and robots.txt. Add a favicon."

Test: use a social media preview tool (like opengraph.xyz) to check that your pages show rich previews when shared.

### Step 10: Polish

> "Claude, review the entire site for consistency. Check: typography is consistent across pages, spacing is uniform, colors match the design system, all links work, images are optimized, the site loads fast. Fix anything that feels off."

> "Claude, test the site on mobile viewport sizes. Fix any layout issues, text overflow, or touch target problems."

### Step 11: Quality Gates

> "Claude, run npm run lint and fix any issues."

> "Claude, run npm run typecheck and fix any issues."

> "Claude, run npm run build and make sure it completes without errors."

### Step 12: Deploy

> "Claude, initialize a git repo, commit all changes, push to GitHub, and deploy to Vercel."

If you have a custom domain:

> "Claude, walk me through adding my custom domain [yourdomain.com] to this Vercel deployment. What DNS records do I need?"

Test on the live URL:
- Visit every page
- Test on your phone
- Share a link somewhere and check the preview card
- Run a Lighthouse audit (Chrome DevTools > Lighthouse) for performance and SEO scores

---

## Acceptance Criteria

Your project is complete when all of these are true:

- [ ] Portfolio site is live on Vercel
- [ ] Home page has hero section with your name and description
- [ ] Projects page showcases at least one project with description and tech stack
- [ ] Blog is set up with MDX and has at least one real post you wrote
- [ ] About page has genuine content about you
- [ ] Contact information is available
- [ ] Every page has proper SEO metadata (title, description, OG tags)
- [ ] Sitemap.xml and robots.txt exist
- [ ] Site is responsive (works on mobile and desktop)
- [ ] Dark mode is supported
- [ ] `npm run lint` passes
- [ ] `npm run typecheck` passes
- [ ] `npm run build` completes without errors
- [ ] Code is on GitHub

---

## Stretch Goals

**Add Animations**
> "Claude, add subtle page transition animations and scroll-triggered animations for project cards and sections. Keep it tasteful -- smooth and fast, not distracting."

**Add a Newsletter Signup**
> "Claude, add a newsletter signup form on the blog page. Integrate with [Buttondown/ConvertKit/Resend] to collect email subscribers."

**Add an RSS Feed**
> "Claude, generate an RSS feed for the blog at /feed.xml so people can subscribe to new posts in their RSS reader."

**Add a Resume/CV Page**
> "Claude, add a /resume page that displays my resume in a clean web format. Also generate a downloadable PDF version."

**Add View Counts**
> "Claude, add view counts to blog posts. Track page views with a simple counter stored in a database (we can use our Neon Postgres). Show the count on the blog index page."

---

## Troubleshooting

**MDX posts aren't rendering**
> "Claude, my blog posts aren't showing up. Check the MDX configuration, the content directory path, and the dynamic route for [slug]. Make sure the frontmatter is being parsed correctly."

**OG images aren't showing in social previews**
> "Claude, when I share my site link, no preview image appears. Check the Open Graph image tags, make sure the image URLs are absolute (include the full domain), and verify the images exist at those URLs."

**Dark mode flashes on load**
> "Claude, when I load the site, it briefly shows light mode before switching to dark mode. Fix the flash of unstyled content by detecting the theme preference before the page renders."

**Site is slow**
> "Claude, the site takes too long to load. Run a Lighthouse audit and optimize: compress images, remove unused CSS, minimize JavaScript, and add proper caching headers."

---

## Reflection

This is the first thing you've built that's entirely yours. Not a tutorial project, not a guided exercise -- your portfolio, your content, your design decisions.

Notice what the process felt like. You researched, you planned, you directed Claude Code, and you ended up with a professional portfolio site. The same process works for any project. The only things that change are the requirements.

Write about this experience. It could be your second blog post.
