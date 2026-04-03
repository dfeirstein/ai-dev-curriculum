# Week 0: Setting Up Your AI Development Environment

> This is your first exercise. By the end, you'll have a fully configured development machine and accounts for every service in the course. Budget a full day (4-6 hours).

This guide is for macOS 13+. Every step is explicit. If you've never opened a terminal before, that's expected — we start there.

---

## Table of Contents

- [Part 1: macOS Basics](#part-1-macos-basics)
- [Part 2: Core Development Tools](#part-2-core-development-tools)
- [Part 3: Your AI Coding Agent](#part-3-your-ai-coding-agent)
- [Part 4: Version Control](#part-4-version-control)
- [Part 5: Deployment Platform](#part-5-deployment-platform)
- [Part 6: Database](#part-6-database)
- [Part 7: Authentication & Payments](#part-7-authentication--payments)
- [Part 8: Email, Monitoring & Analytics](#part-8-email-monitoring--analytics)
- [Part 9: File Storage](#part-9-file-storage)
- [Part 10: Verification Exercise](#part-10-verification-exercise)
- [Troubleshooting](#troubleshooting)

---

## Part 1: macOS Basics

Before we install anything, let's get comfortable with your Mac as a developer machine.

### What is a Terminal?

A terminal is a text-based interface to your computer. Instead of clicking icons and buttons, you type commands. Every developer uses a terminal — it's faster, more precise, and it's where Claude Code (your AI assistant) lives.

**Open the built-in Terminal:**
1. Press `Cmd + Space` (this opens Spotlight search)
2. Type `Terminal`
3. Press `Enter`

A window appears with a blinking cursor. This is your terminal. You type commands here and press `Enter` to run them.

### Your First Commands

Type each of these and press `Enter`. Watch what happens.

```bash
pwd
```
This prints "where you are" on your computer — your current directory. You'll see something like `/Users/yourname`.

```bash
ls
```
This lists what's in your current directory — your files and folders.

```bash
mkdir test-folder
```
This creates a new folder called `test-folder`.

```bash
cd test-folder
```
This moves you into that folder. Run `pwd` again — notice the path changed.

```bash
cd ..
```
This moves you back up one level.

```bash
rmdir test-folder
```
This removes the empty folder you created.

### Key Commands You'll Use

| Command | What It Does | Example |
|---------|-------------|---------|
| `pwd` | Where am I? | `pwd` → `/Users/yourname` |
| `ls` | What's here? | `ls` → lists files and folders |
| `cd` | Go into a folder | `cd Documents` |
| `cd ..` | Go back up | `cd ..` |
| `mkdir` | Create a folder | `mkdir my-project` |
| `clear` | Clear the screen | `clear` |
| `Ctrl + C` | Cancel a running command | — |

### macOS Security Settings

macOS sometimes blocks apps from developers who aren't Apple. You'll encounter this when installing tools. Here's how to handle it:

1. If you see "App can't be opened because it's from an unidentified developer":
   - Go to **System Settings → Privacy & Security**
   - Scroll down — you'll see a message about the blocked app
   - Click **Open Anyway**

2. Some terminal commands ask for your Mac password:
   - When you type your password in the terminal, **no characters appear** — that's normal, it's a security feature
   - Type your password and press `Enter`

### Exercise: Create Your Projects Folder

Every project in this course will live in one place. Let's create it:

```bash
mkdir -p ~/projects
cd ~/projects
pwd
```

You should see `/Users/yourname/projects`. This is home base for the next 20 weeks.

---

## Part 2: Core Development Tools

### Install Homebrew

Homebrew is a package manager — it installs developer tools with a single command instead of downloading from websites.

```bash
/bin/bash -c "$(curl -fsSL https://raw.githubusercontent.com/Homebrew/install/HEAD/install.sh)"
```

This takes a few minutes. It will:
- Ask for your Mac password (remember: no characters show when typing)
- Download and install Homebrew

**When it finishes**, read the output carefully. It will tell you to run two commands to add Homebrew to your PATH. They look like:

```bash
echo 'eval "$(/opt/homebrew/bin/brew shellenv)"' >> ~/.zprofile
eval "$(/opt/homebrew/bin/brew shellenv)"
```

Run whatever commands it shows you.

**Verify:**
```bash
brew --version
```

You should see `Homebrew 4.x.x`. If you see "command not found", close the terminal, reopen it, and try again.

### Install Ghostty (Your Terminal)

Ghostty is a modern terminal. It's fast, beautiful, and what you'll use for the rest of the course.

1. Go to https://ghostty.org
2. Click **Download for macOS**
3. Open the downloaded `.dmg` file
4. Drag Ghostty to your Applications folder
5. Open Ghostty from Applications
   - First time: you may need to right-click → Open (macOS security)

**Configure Ghostty** — make it look good:

```bash
mkdir -p ~/.config/ghostty
```

```bash
cat > ~/.config/ghostty/config << 'EOF'
font-family = JetBrains Mono
font-size = 14
theme = catppuccin-mocha
window-padding-x = 10
window-padding-y = 10
EOF
```

Install the JetBrains Mono programming font:
```bash
brew install --cask font-jetbrains-mono
```

**Close the built-in Terminal app. Open Ghostty. Use Ghostty from now on.**

Restart Ghostty to see the new theme and font take effect.

### Install Zed (Your Code Editor)

Zed is where you'll review the files Claude Code creates. Think of it as a window into your project.

```bash
brew install --cask zed
```

Open Zed for the first time:
1. Launch it from Applications
2. When it asks to install the CLI tool, say **Yes** (this lets you type `zed .` in the terminal to open the current folder)
3. If it asks to sign in, you can skip this for now

**Verify:**
```bash
zed --version
```

### Install Node.js

Node.js runs JavaScript and TypeScript. It's the engine behind everything you'll build.

We install it through **nvm** (Node Version Manager), which lets you switch Node versions easily:

```bash
curl -o- https://raw.githubusercontent.com/nvm-sh/nvm/v0.40.1/install.sh | bash
```

**Close Ghostty and reopen it** (nvm needs a fresh terminal), then:

```bash
nvm install --lts
```

This installs the latest stable version of Node.js.

**Verify:**
```bash
node --version
npm --version
```

You should see version numbers (like `v22.x.x` and `10.x.x`).

Install **pnpm** (a faster alternative to npm):
```bash
npm install -g pnpm
```

### Checkpoint: Core Tools

Run this to verify everything so far:
```bash
echo "=== Core Tools Check ===" && brew --version && echo "---" && node --version && npm --version && pnpm --version && echo "---" && zed --version && echo "=== All core tools installed ==="
```

Every line should show a version. If anything says "command not found", go back to that step.

---

## Part 3: Your AI Coding Agent

### Install Claude Code CLI

Claude Code is the most important tool in this course. It's an AI coding agent that lives in your terminal. You describe what you want, and it builds it.

```bash
npm install -g @anthropic-ai/claude-code
```

### Set Up Your Anthropic Account

1. Go to https://console.anthropic.com
2. Create an account (or sign in)
3. You need API access — add a payment method and get an API key
4. Keep this tab open — you'll need it in a moment

### Authenticate Claude Code

```bash
claude
```

This starts Claude Code for the first time. It will walk you through authentication:
- Follow the prompts to connect your Anthropic account
- It may open a browser for authentication

Once authenticated, you'll see Claude Code's prompt. Type `exit` to leave for now.

**Test it:**
```bash
claude "What is 2 + 2? Reply in one word."
```

If you get a response, Claude Code is working.

### What You'll Use Claude Code For

Throughout this course, Claude Code is your primary tool. You'll:
- Start it with `claude` in any project directory
- Describe what you want built in plain English
- Review what it creates
- Iterate until it matches your vision
- Ask it to commit, push, deploy

You don't write code. You direct Claude Code.

### Claude Code Modes — Your Gearshift

Claude Code has three modes you toggle with **`Shift+Tab`**:

- **Normal mode** — Claude builds with full autonomy (reads, writes, runs commands)
- **Plan mode** — Claude researches and plans but doesn't change any files
- **Accept-edits mode** — Claude proposes each change, you approve one by one

You'll learn when to use each mode in Week 1. For now, just know that `Shift+Tab` is the most important keyboard shortcut.

### Install the Claude Chrome Extension

The Chrome extension lets Claude Code see and interact with your web browser. This is useful for testing your apps, verifying dashboards, and automating tedious browser tasks.

1. Open Chrome (install it from https://google.com/chrome if you don't have it)
2. Go to the Chrome Web Store and search for "Claude Code" (or ask Claude Code: "How do I install the Chrome extension?")
3. Install the extension
4. You'll see the Claude icon in your Chrome toolbar
5. Click it to open the **Side Panel** — this is where you can give Claude instructions directly in the browser

**Two ways to use it:**
- **From Ghostty (CLI):** Tell Claude Code "open Chrome and navigate to my Vercel dashboard"
- **From Chrome (Side Panel):** Open the Side Panel and type "verify this page loaded correctly"

**Test it now:**
After setting up your Vercel account (Part 5), try:
- Open Chrome and navigate to https://vercel.com/dashboard
- Open the Claude Side Panel
- Type: "What do you see on this page?"

If Claude can describe the page content, the extension is working.

---

## Part 4: Version Control

### Install and Configure Git

Git tracks every change to your project. Think of it as unlimited undo — you can always go back to any previous version.

```bash
brew install git
```

**Configure Git with your identity:**
```bash
git config --global user.name "Your Full Name"
git config --global user.email "your.email@example.com"
git config --global init.defaultBranch main
```

Use the same email you'll use for GitHub.

### Generate SSH Keys

SSH keys let you securely connect to GitHub without typing your password every time.

```bash
ssh-keygen -t ed25519 -C "your.email@example.com"
```

Press `Enter` three times to accept all defaults (no passphrase).

Copy your public key to the clipboard:
```bash
pbcopy < ~/.ssh/id_ed25519.pub
```

### Create Your GitHub Account

GitHub is where your code lives on the internet. It's your portfolio, your collaboration platform, and your shipping pipeline.

1. Go to https://github.com
2. Create an account
   - **Choose your username carefully** — this will be on your portfolio and resume
   - Use a professional name, not a joke handle
3. Go to **Settings** (click your avatar → Settings)
4. Click **SSH and GPG keys** in the left sidebar
5. Click **New SSH key**
   - Title: "My Mac"
   - Key type: Authentication Key
   - Paste the key you copied (it's still on your clipboard from `pbcopy`)
6. Click **Add SSH key**

**Test the connection:**
```bash
ssh -T git@github.com
```

Type `yes` when it asks about the fingerprint. You should see: "Hi username! You've successfully authenticated..."

### Install GitHub CLI

The GitHub CLI (`gh`) lets you create repos, pull requests, and more from your terminal.

```bash
brew install gh
gh auth login
```

When prompted, choose:
- GitHub.com
- SSH
- Your existing SSH key
- Login with a web browser

It will open your browser — authorize the CLI.

**Verify:**
```bash
gh auth status
```

You should see "Logged in to github.com."

---

## Part 5: Deployment Platform

### Create Your Vercel Account

Vercel is where your applications get deployed. Push code to GitHub → Vercel automatically puts it on the internet.

1. Go to https://vercel.com
2. Click **Sign Up** → **Continue with GitHub**
3. Authorize Vercel to access your GitHub account
4. Complete the onboarding

### Explore the Vercel Dashboard

Take a minute to look around:
- **Overview** — shows your deployed projects (empty for now)
- **Settings** — account configuration
- **Storage** — Vercel Blob and other storage options (we'll use this later)

### Install Vercel CLI

```bash
npm install -g vercel
vercel login
```

Choose "Continue with GitHub" and follow the browser flow.

**Verify:**
```bash
vercel --version
```

### What Vercel Does For You

Vercel handles things you don't want to think about:
- **Automatic deploys**: Push to GitHub → live on the internet in seconds
- **Preview deployments**: Every pull request gets its own URL for testing
- **Custom domains**: Use your own domain name (e.g., yourname.com)
- **Environment variables**: Store secrets securely (API keys, database passwords)
- **Analytics**: Built-in performance monitoring
- **Edge network**: Your app loads fast from anywhere in the world

You'll use all of these features throughout the course.

---

## Part 6: Database

### Create Your Neon Account

Neon is hosted PostgreSQL — a database in the cloud. You don't install or manage anything. They give you a connection string, and your app stores data there.

1. Go to https://neon.tech
2. Click **Sign Up** → sign in with GitHub
3. Create your first project:
   - Name: `ai-native-course`
   - Region: pick the closest to you
   - Postgres version: latest (default is fine)
4. **Save your connection string** — it looks like: `postgresql://user:password@endpoint.neon.tech/dbname?sslmode=require`
   - You'll need this in Week 5. Store it somewhere safe (Notes app, password manager, etc.)
   - **Never share this string** — it's like a password to your database

### What You Need to Know About Databases

You don't need to learn SQL. You need to understand the concept:
- A database stores your application's data (users, projects, tasks, etc.)
- Data is organized into **tables** (like spreadsheets)
- Tables have **rows** (entries) and **columns** (fields)
- Your app reads and writes to the database through an **ORM** (Drizzle) — Claude Code handles this

---

## Part 7: Authentication & Payments

### Better Auth

Better Auth handles user accounts for your apps. It doesn't require a separate account — it runs as part of your application using your own database. When the time comes (Week 6), you'll tell Claude Code to integrate it.

**No account setup needed.** Just know it exists and what it does:
- Email/password signup and login
- OAuth (Login with GitHub, Google)
- Session management
- Password reset

### Create Your Stripe Account

Stripe handles payments. You'll use it to add subscriptions to your SaaS.

1. Go to https://stripe.com
2. Click **Start now** → create an account
3. You can skip business verification for now — you'll be using **test mode** only
4. Once in the dashboard:
   - Look for the **Test mode** toggle in the top right — make sure it's **ON** (orange)
   - Go to **Developers** → **API keys**
   - You'll see a **Publishable key** and a **Secret key** — you'll need both in Week 9
   - **Never share your secret key**

### What You Need to Know About Stripe

- **Test mode** means no real money changes hands. You use fake credit card numbers.
- **Checkout** is Stripe's hosted payment page — you send users there to pay
- **Webhooks** are how Stripe tells your app that something happened (payment succeeded, subscription canceled, etc.)
- **Customer Portal** lets users manage their own subscription (upgrade, downgrade, cancel)

You'll direct Claude Code to set all of this up in Week 9.

---

## Part 8: Email, Monitoring & Analytics

### Create Your Resend Account

Resend sends transactional emails (welcome emails, notifications, password resets).

1. Go to https://resend.com
2. Sign up with GitHub
3. In the dashboard:
   - Go to **API Keys** → create an API key
   - Save it somewhere safe — you'll need it in Week 10
   - The free tier gives you 3,000 emails/month (more than enough)

### Create Your Sentry Account

Sentry catches errors in production. When something breaks, Sentry captures it with full context so you can tell Claude Code to fix it.

1. Go to https://sentry.io
2. Sign up with GitHub
3. When it asks you to create a project:
   - Platform: **Next.js**
   - Project name: `teamtask-pro` (or skip and do this later)
4. In the dashboard:
   - Find your **DSN** (Data Source Name) — it's the connection URL for Sentry. Looks like: `https://examplePublicKey@o0.ingest.sentry.io/0`
   - You'll need this in Week 13
5. Explore the dashboard briefly:
   - **Issues** — where captured errors appear
   - **Performance** — loading speed and web vitals
   - **Alerts** — configure notifications when error rates spike

### What Sentry Does For You

When your app is live and a user encounters an error:
- Sentry captures the error automatically
- Records what the user was doing, what browser they used, the full error trace
- Groups similar errors together so you're not overwhelmed
- Alerts you if errors spike after a deploy
- You copy the error context and paste it to Claude Code: "Fix this production error"

### Create Your PostHog Account

PostHog tracks how users use your app — what they click, where they drop off, which features get used.

1. Go to https://posthog.com
2. Sign up with GitHub
3. In the onboarding:
   - Create a project: `teamtask-pro`
   - Choose **Cloud** (not self-hosted)
4. Find your **API key** (also called project key) in **Settings → Project → API Key**
   - You'll need this in Week 13
5. Explore the dashboard briefly:
   - **Events** — raw stream of user actions
   - **Insights** — charts and funnels you build from events
   - **Session Recordings** — watch replays of user sessions
   - **Feature Flags** — release features to a subset of users
   - **Experiments** — A/B test changes

### What PostHog Does For You

PostHog answers questions like:
- How many users signed up this week?
- What percentage of users who sign up actually create their first project?
- Which features do paying users use most?
- Where do users drop off in the onboarding flow?

This data helps you make better product decisions. You'll set it up in Week 13.

---

## Part 9: File Storage

Your apps will need to store files — profile images, document uploads, attachments. You need a cloud storage service for this.

### Vercel Blob (Recommended for This Course)

Vercel Blob is the simplest option — it's built into Vercel, where you're already deploying.

1. Go to your Vercel dashboard → **Storage**
2. Click **Create** → **Blob**
3. Name it: `ai-native-course-files`
4. Accept the defaults
5. Vercel will add a `BLOB_READ_WRITE_TOKEN` to your project's environment variables automatically

**What Vercel Blob does:** Stores files (images, documents, etc.) in the cloud and gives you URLs to access them. Simple API, no configuration, works seamlessly with your Vercel deployments.

### Alternatives You Should Know About

You won't use all of these, but you should know they exist so you can direct Claude Code effectively:

| Service | What It Does | When to Choose It |
|---------|-------------|-------------------|
| **Vercel Blob** | Simple file storage, built into Vercel | Default for this course — simplest setup |
| **Uploadthing** | File uploads with built-in UI components | When you want drag-and-drop upload UIs out of the box |
| **AWS S3** | Industry-standard cloud storage | When you need maximum flexibility or your company uses AWS |
| **Cloudflare R2** | S3-compatible, no egress fees | When you serve lots of files and want to save on bandwidth costs |
| **Google Cloud Storage (GCS)** | Google's cloud storage | When you're in the Google Cloud ecosystem |

For this course, Vercel Blob handles everything you need. When directing Claude Code, you'll say: "Use Vercel Blob for file storage."

### The Mental Model for File Storage

File storage works like this:
1. User selects a file (image, document, etc.)
2. Your app uploads the file to cloud storage (Vercel Blob, S3, etc.)
3. Cloud storage returns a URL for that file
4. You save the URL in your database (not the file itself)
5. When displaying the file, you use the URL

The database stores the *reference* to the file. The cloud stores the *actual* file. This is important — your database stays small and fast.

---

## Part 10: Verification Exercise

Let's verify your entire setup works end-to-end. This exercise tests: terminal, Git, GitHub, Claude Code, and Vercel.

### Step 1: Create a Test Project

Open Ghostty and run:

```bash
cd ~/projects
mkdir setup-test && cd setup-test
```

### Step 2: Use Claude Code to Build Something

```bash
claude
```

Once Claude Code is active, type:

> Create a simple Next.js page that says "My dev environment is ready!" with today's date. Use TypeScript and Tailwind.

Let Claude Code create the files. When it's done:

> Run the dev server so I can see it.

Open the URL it gives you (usually http://localhost:3000). You should see your page.

### Step 3: Push to GitHub

Tell Claude Code:

> Initialize a git repo, commit everything with the message "setup verification", create a public GitHub repo called setup-test, and push.

Verify it worked: `gh repo view --web` should open your new repo on GitHub.

### Step 4: Deploy to Vercel

Tell Claude Code:

> Deploy this to Vercel.

When it's done, you'll get a live URL. Open it — your page should be live on the internet.

### Step 5: Clean Up

```bash
cd ~/projects
rm -rf setup-test
gh repo delete setup-test --yes
```

### Checkpoint: Full Setup Verification

Run this final check:

```bash
echo "=== Full Setup Verification ==="
echo ""
echo "Core tools:"
brew --version | head -1
node --version
npm --version
git --version | head -1
echo ""
echo "Development tools:"
zed --version 2>/dev/null || echo "Zed: check manually"
claude --version 2>/dev/null || echo "Claude Code: check manually"
vercel --version 2>/dev/null || echo "Vercel CLI: check manually"
echo ""
echo "Accounts (verify manually):"
gh auth status 2>&1 | head -2
echo "Neon: https://console.neon.tech"
echo "Stripe: https://dashboard.stripe.com (test mode)"
echo "Resend: https://resend.com/api-keys"
echo "Sentry: https://sentry.io"
echo "PostHog: https://app.posthog.com"
echo "Vercel: https://vercel.com/dashboard"
echo ""
echo "=== Setup complete ==="
```

### Accounts Checklist

Make sure you have accounts created and API keys saved for:

- [ ] **GitHub** — account created, SSH key added, GH CLI authenticated
- [ ] **Anthropic** — account with API access, Claude Code authenticated
- [ ] **Vercel** — account created (GitHub sign-in), CLI authenticated
- [ ] **Neon** — database created, connection string saved
- [ ] **Stripe** — account created, in test mode, API keys noted
- [ ] **Resend** — account created, API key saved
- [ ] **Sentry** — account created, Next.js project set up, DSN noted
- [ ] **PostHog** — account created, project created, API key noted
- [ ] **Vercel Blob** — blob store created in Vercel dashboard

Save all your API keys and connection strings somewhere safe (password manager recommended). You'll need them throughout the course.

---

## Troubleshooting

### "command not found"
- Close Ghostty and reopen it (shell needs to reload)
- Make sure Homebrew's PATH is set up (Part 2)
- Try: `source ~/.zshrc`

### macOS blocks an application
- System Settings → Privacy & Security → scroll down → Open Anyway

### Permission errors with npm
- Don't use `sudo` with npm — if you're getting permission errors, reinstall Node via nvm
- `nvm install --lts` (from Part 2)

### SSH key issues with GitHub
- Start the SSH agent: `eval "$(ssh-agent -s)"`
- Add your key: `ssh-add ~/.ssh/id_ed25519`
- Verify: `ssh -T git@github.com`

### Claude Code won't authenticate
- Make sure you have an Anthropic account with API access at https://console.anthropic.com
- Try: `claude logout` then `claude` again

### "Port 3000 already in use"
- Another dev server is running. Kill it: `npx kill-port 3000`

---

## What's Next

Your machine is fully set up. Every tool is installed. Every account is created. You're ready.

Go to **[Week 1](./week-01/README.md)** and build your first project.
