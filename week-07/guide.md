# Week 7 Guide: Personal Portfolio + Blog

You have the tools. You have the workflow. Now you need something to show for it.

A portfolio is the single most important asset for someone building with technology. It's proof of work. It's your public identity. It's the thing you send when someone asks "what have you built?" This week you'll research what makes portfolios great, then direct Claude Code to build yours.

---

## Part 1: Research -- What Makes a Great Portfolio?

Before you build anything, study what already works. This is the research phase of your workflow, and for a creative project like this, it matters even more than usual.

### What to Look For

Spend 30-60 minutes browsing developer and builder portfolios. You're not copying -- you're identifying patterns. Notice:

**Structure.** Most great portfolios have a small number of pages: Home (hero + summary), Projects (what you've built), About (who you are), Blog (what you think), and Contact (how to reach you). Some combine these into a single long page. Others use separate pages. Neither is wrong.

**First impression.** You have about three seconds before someone decides to stay or leave. What do the best portfolios show in those three seconds? Usually: a name, a one-line description of what the person does, and a clear visual identity.

**Project showcases.** The best portfolios don't just list projects -- they tell the story. What was the problem? What did you build? What did you learn? Screenshots or live links make it real.

**Writing voice.** Blog posts on great portfolios sound like a person, not a textbook. They share genuine experiences, lessons learned, and honest reflections. Authenticity beats polish.

**Design.** Clean, readable, fast. The best portfolios aren't the most complex -- they're the most clear. Whitespace, typography, and a limited color palette go a long way.

### When Directing Claude Code

> "Claude, I'm researching portfolio site structures. What are the most common page layouts for developer portfolios? What sections do the best ones include?"

---

## Part 2: Content Strategy

A portfolio is only as good as its content. The technology is just a delivery mechanism.

### What to Include

**Projects.** Even if you've only built the bookmark manager so far, that counts. Describe what it does, what stack it uses, and what you learned building it. As you build more projects in this curriculum, you'll add them here.

**About.** Who are you? What's your background? Why are you learning to build software? This doesn't need to be long -- two or three paragraphs that give someone a sense of who you are. Be genuine.

**Blog posts.** Write about what you're learning. Your first post could be about your experience learning to build with AI. Future posts can cover specific projects, concepts you found interesting, or tools you discovered. Writing crystallizes your understanding and builds your reputation.

**Contact.** How can people reach you? Email, GitHub, LinkedIn, Twitter/X -- whatever you're comfortable sharing.

### The "Building in Public" Mindset

Building in public means sharing your work and your process openly. It's one of the most effective ways to build a reputation, attract opportunities, and connect with other builders.

What this looks like in practice:
- Write a blog post about each project you build
- Share your portfolio link on social media
- Be honest about what you're learning and what you don't know yet
- Show your work, not just your results

The people who succeed in tech are not the ones who know the most -- they're the ones who share the most. Your portfolio and blog are the foundation for that.

### When Directing Claude Code

> "Claude, I need to plan the content for my portfolio. Here's my background: [describe yourself]. Here are the projects I've built: [list them]. Help me outline an About page, project descriptions, and three blog post ideas."

---

## Part 3: SEO -- Making Your Site Findable

SEO (Search Engine Optimization) is how search engines like Google find, understand, and rank your site. You don't need to become an SEO expert, but you should understand the basics and direct Claude Code to implement them.

### Metadata

Every page on the web has metadata -- information about the page that isn't visible to users but is read by search engines and social platforms.

**Title tag.** The text that appears in the browser tab and in search results. Each page should have a unique, descriptive title. "John Doe -- Software Builder" is better than "Home."

**Meta description.** A brief summary of the page that appears in search results below the title. One or two sentences that tell someone what they'll find on the page.

### Open Graph

When you share a link on Twitter, LinkedIn, or Slack, it shows a preview card with a title, description, and image. That preview comes from Open Graph (OG) tags -- metadata specifically for social sharing.

If you don't set OG tags, your link shows up as plain text. If you do, it shows a rich, clickable card that gets more attention.

### Sitemaps

A sitemap is a file that lists every page on your site. Search engines use it to discover and index your content. It's an XML file at `/sitemap.xml`. Next.js can generate this automatically.

### When Directing Claude Code

> "Claude, add proper SEO to every page: unique title tags, meta descriptions, and Open Graph tags with images. Generate a sitemap.xml. Add a robots.txt file."

---

## Part 4: MDX -- Markdown with Components

Your blog posts need a content format. MDX is the best choice for a Next.js blog.

### What MDX Is

You already know Markdown from your CLAUDE.md files and GitHub README files. It's the format with `# headings`, `**bold text**`, and `- bullet lists`.

MDX is Markdown + Components. It's Markdown that can also include interactive React components. A regular Markdown blog post can have text, headings, images, and code blocks. An MDX blog post can have all of that plus interactive charts, embedded demos, custom callout boxes, or anything else you can build as a component.

### How It Works in Practice

You write blog posts as `.mdx` files. Each file has frontmatter (metadata at the top, like title and date) and content (your writing). Next.js reads these files and renders them as pages.

```
---
title: "My First Blog Post"
date: "2026-03-15"
description: "What I learned building software with AI"
---

Here's what I learned this week...
```

The key insight: you don't need to know how MDX works under the hood. You need to know that it lets you write blog posts as simple text files, and Claude Code knows how to set it up in a Next.js project.

### When Directing Claude Code

> "Claude, set up MDX for blog posts. Each post should be a .mdx file in a /content/blog directory with frontmatter for title, date, description, and tags. Create an index page at /blog that lists all posts sorted by date, and individual post pages at /blog/[slug]."

---

## Part 5: Custom Domains

Your site is currently at `your-app.vercel.app`. That works, but a custom domain makes it professional. `yourname.com` or `yourname.dev` is more memorable and more credible.

### How Custom Domains Work

A domain name (like `yourname.com`) is just a human-readable address. Behind it is a DNS (Domain Name System) record that tells browsers where to actually go. When you connect a domain to Vercel, you're telling DNS: "When someone types `yourname.com`, send them to my Vercel deployment."

Vercel handles the SSL certificate (the padlock icon that means the connection is secure) automatically. You just need to point your domain's DNS records to Vercel.

### The Process

1. Buy a domain from a registrar (Namecheap, Cloudflare, Google Domains)
2. In Vercel, go to your project settings and add the domain
3. Vercel gives you DNS records to add at your registrar
4. Add the records, wait a few minutes for propagation
5. Your site is live on your custom domain with HTTPS

This is optional. `your-app.vercel.app` is perfectly fine for now. But if you want to build a professional presence, a custom domain is worth the small investment.

### When Directing Claude Code

> "Claude, walk me through connecting a custom domain to my Vercel deployment. What DNS records do I need to set up?"

---

## Part 6: Putting It All Together

Here's what your portfolio project combines:

- **Next.js** for the application framework
- **TypeScript** for code quality
- **Tailwind + shadcn/ui** for design and components
- **MDX** for blog content
- **SEO metadata** for discoverability
- **Vercel** for deployment
- The **research, plan, execute** workflow for the entire build

This is your first real creative project. You're not following step-by-step instructions to build a predefined app. You're making decisions about design, content, and structure -- then directing Claude Code to execute your vision.

---

## What's Next

Head to [project.md](project.md) to build your portfolio and blog.
