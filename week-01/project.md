# Project: Personal Landing Page

**What you're building:** A personal landing page with your name, bio, and links -- deployed to a live URL anyone can visit.

**Time estimate:** 4-6 hours (including reading, experimenting, and deploying)

**What you'll need open:** Ghostty (your terminal) and a web browser

---

## The Brief

You're going to build a single-page personal website. It should feel like a digital business card: clean, professional, and distinctly yours. When you're done, you'll have a live URL you can share with anyone.

You won't write any code. You'll direct Claude Code to build everything. Your job is to describe what you want, review the result, give feedback, and ship it.

---

## Step-by-Step

### Step 1: Set Up Your Workspace

Open Ghostty. Create a folder for your projects and navigate into it:

```
mkdir -p ~/projects && cd ~/projects
```

This is where all your projects will live. One folder per project, everything organized.

### Step 2: Start Claude Code

```
claude
```

You're now talking to your AI builder. Everything from here is a conversation.

### Step 3: Create the Project

Give Claude Code your first prompt:

> "Create a new Next.js project called my-landing-page with TypeScript, Tailwind CSS, and the App Router. Use the latest version."

Claude Code will create the project and install everything needed. This might take a minute. When it's done, you'll have a full project structure ready to go.

### Step 4: Take a Look Around

Open the project in Zed to see what Claude Code created:

```
zed .
```

You'll see a bunch of files and folders. Don't worry about understanding all of them. Here's what matters right now:

- There's a project with a bunch of configuration files (normal -- every project has these)
- There's a folder structure that organizes everything
- Somewhere in there is the page that will become your landing page

If you're curious, ask Claude Code: "Give me a quick overview of the project structure and what the important files are."

### Step 5: See It Running

Tell Claude Code:

> "Run the dev server so I can see the site in my browser"

Claude Code will start a development server and give you a URL, usually `http://localhost:3000`. Open that in your browser. You should see the default Next.js starter page. It's not yours yet -- but it will be.

### Step 6: Make It Yours

This is where it gets fun. Tell Claude Code what you want your landing page to look like. Here's a starting prompt -- customize it with your actual information:

> "Replace the default homepage with a personal landing page. My name is [YOUR NAME]. Include: a headline with my name, a short tagline about who I am (something like 'Builder, learner, maker of things'), links to my GitHub (github.com/YOURUSERNAME), LinkedIn (linkedin.com/in/YOURUSERNAME), and email (your@email.com). Use a clean, modern design with good typography, centered layout, and plenty of white space."

Check your browser. The page should update automatically. Look at it and ask yourself:

- Does it have my name?
- Does the tagline feel right?
- Are the links there and do they point to the right places?
- Does it look clean and professional?

### Step 7: Refine It

Almost nobody gets exactly what they want on the first try. That's normal. Look at the page and tell Claude Code what to adjust. Here are some examples of the kinds of feedback you might give:

> "Make the heading larger and bolder"

> "Add more space between the tagline and the links"

> "Change the link color to something that stands out more"

> "The layout looks off on mobile -- make sure it works well on small screens too"

> "Add a subtle background color -- something warm and inviting"

Go back and forth a few times. Each round will get you closer to something you're proud of. This iterative process IS the workflow. It's not a sign that something went wrong -- it's how building works.

### Step 8: Save Your Work with Git

Once you're happy with how it looks (or happy enough for now -- you can always change it later), tell Claude Code:

> "Initialize a git repository, commit all the files with the message 'Initial landing page with bio and links', create a public GitHub repo called my-landing-page, and push everything"

Claude Code will handle the Git setup, create the repository on GitHub, and push your code. When it's done, your code is backed up in the cloud.

Verify by asking Claude Code: "What's the GitHub URL for this repo?" Open it in your browser and make sure you can see your project there.

### Step 9: Deploy to Vercel

Time to go live. Tell Claude Code:

> "Deploy this project to Vercel"

Claude Code will connect your GitHub repo to Vercel and trigger a deployment. This might involve a few steps -- follow Claude Code's guidance if it asks you to authorize anything.

When deployment is complete, you'll get a URL. Something like `my-landing-page.vercel.app`.

### Step 10: Verify and Share

Open your live URL in a browser. Check everything:

- Does the page load?
- Does it look the same as it did on localhost?
- Do all the links work?
- Try it on your phone too -- does it look good on a small screen?

If something's wrong, go back to Claude Code, describe the issue, fix it, commit, and push. Vercel will automatically redeploy.

Now share that URL with someone. You just built and deployed a website.

---

## Acceptance Criteria

Your project is complete when all of these are true:

- [ ] Your landing page is live at a public URL (Vercel)
- [ ] It shows your name and a bio or tagline
- [ ] It includes at least 3 links (GitHub, LinkedIn, email, or others)
- [ ] The design is clean and professional
- [ ] Your code is on GitHub in a public repository
- [ ] Running `npm run build` completes without errors

---

## Stretch Goals

Finished early? Want to push further? Try these:

**Add a Dark Mode Toggle**
> "Add a dark mode toggle button. When clicked, the page should switch between a light theme and a dark theme. Remember the user's preference."

**Add a Projects Section**
> "Add a section below my bio called 'Projects' with 3 placeholder project cards. Each card should have a title, a one-line description, and a link. Use a grid layout."

**Add an Animated Background**
> "Add a subtle animated gradient background that slowly shifts between two or three colors. Keep it smooth and not distracting."

**Add a Favicon**
> "Create a simple favicon for my site -- just my initials in a clean font on a colored background."

For each stretch goal, remember the workflow: prompt Claude Code, review the result, give feedback, test it, commit, and push. Vercel will deploy your updates automatically.

---

## Troubleshooting

**The dev server won't start**
Tell Claude Code: "The dev server isn't starting. Here's the error I see: [paste the error]. Fix it."

**The page looks broken in the browser**
Describe what you see: "The page is showing raw text instead of a styled layout" or "Everything is bunched up on the left side" or whatever you're seeing. Be specific.

**Git push failed**
Tell Claude Code: "Git push failed with this error: [paste the error]. Help me fix it."

**Vercel deployment failed**
Tell Claude Code: "The Vercel deployment failed. Here's the error from the Vercel dashboard: [paste or describe the error]. What's wrong?"

The pattern is always the same: describe what went wrong as specifically as you can, and let Claude Code figure out the fix. You don't need to diagnose the problem yourself. You just need to notice it and communicate it clearly.

---

## Reflection

When you're done, take a minute to think about what just happened:

- You built a website without writing code.
- You deployed it to the internet without configuring a server.
- You used version control without memorizing Git commands.
- You directed an AI to do the technical work while you focused on the creative decisions.

This is the new normal. And it's just the beginning.
