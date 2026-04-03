# Machine Setup

> Install everything you need. Budget 60-90 minutes.

This guide is for macOS 13+. Every step is explicit. If something fails, ask Claude Code (once it's installed) — that's the whole point.

---

## 1. Open Terminal

Press `Cmd + Space`, type `Terminal`, press Enter. This is temporary — you'll switch to Ghostty shortly.

**Four commands to know right now:**
- `pwd` — where am I?
- `ls` — what's here?
- `cd folder-name` — go into a folder
- `cd ..` — go back up

---

## 2. Install Homebrew

Homebrew installs developer tools. One command:

```bash
/bin/bash -c "$(curl -fsSL https://raw.githubusercontent.com/Homebrew/install/HEAD/install.sh)"
```

Enter your Mac password when asked (you won't see characters — that's normal). When it finishes, run whatever "Next steps" commands it shows you — usually something like:

```bash
echo 'eval "$(/opt/homebrew/bin/brew shellenv)"' >> ~/.zprofile
eval "$(/opt/homebrew/bin/brew shellenv)"
```

Verify: `brew --version`

---

## 3. Install Ghostty

Your terminal for the rest of the course.

1. Go to https://ghostty.org → Download for macOS
2. Drag to Applications
3. Open it (right-click → Open if macOS blocks it)

Set up a nice config:
```bash
mkdir -p ~/.config/ghostty
cat > ~/.config/ghostty/config << 'EOF'
font-family = JetBrains Mono
font-size = 14
theme = catppuccin-mocha
window-padding-x = 10
window-padding-y = 10
EOF
```

Install the font:
```bash
brew install --cask font-jetbrains-mono
```

**Close the built-in Terminal. Use Ghostty from now on.**

---

## 4. Install Zed

Your code editor. This is where you'll review what Claude Code builds.

```bash
brew install --cask zed
```

Open Zed, let it install the CLI tool when prompted. Verify: `zed --version`

---

## 5. Install Node.js

Node.js runs JavaScript/TypeScript. You need it for everything.

```bash
curl -o- https://raw.githubusercontent.com/nvm-sh/nvm/v0.40.1/install.sh | bash
```

**Close and reopen Ghostty**, then:

```bash
nvm install --lts
npm install -g pnpm
```

Verify: `node --version` and `npm --version`

---

## 6. Install Git

Git tracks changes to your code. Think: save points in a video game.

```bash
brew install git
git config --global user.name "Your Full Name"
git config --global user.email "your.email@example.com"
git config --global init.defaultBranch main
```

Generate an SSH key (for secure GitHub access):
```bash
ssh-keygen -t ed25519 -C "your.email@example.com"
```

Press Enter three times (accept all defaults). Then copy the key:
```bash
pbcopy < ~/.ssh/id_ed25519.pub
```

---

## 7. GitHub Account + CLI

1. Create an account at https://github.com (if you don't have one)
2. Go to Settings → SSH and GPG keys → New SSH key → paste what you copied
3. Verify: `ssh -T git@github.com`

Install the GitHub CLI:
```bash
brew install gh
gh auth login
```

Choose: GitHub.com → SSH → your existing key → web browser login.

---

## 8. Install Claude Code CLI

This is your AI coding agent. It lives in your terminal.

```bash
npm install -g @anthropic-ai/claude-code
```

Run `claude` and follow the authentication prompts. You need an Anthropic account with API access.

Test it: `claude "What is 2 + 2?"` — if you get a response, you're set.

---

## 9. Install Vercel CLI

Vercel deploys your apps to the internet.

1. Create an account at https://vercel.com (sign up with GitHub)
2. Install the CLI:

```bash
npm install -g vercel
vercel login
```

Verify: `vercel --version`

---

## 10. Create Accounts (For Later Weeks)

You don't need these yet. Create the accounts now so you're ready:

| Service | URL | Free Tier | Used In |
|---------|-----|-----------|---------|
| **Neon** (Postgres) | https://neon.tech | Yes | Week 5 |
| **Stripe** (Payments) | https://stripe.com | Test mode | Week 9 |
| **Resend** (Email) | https://resend.com | 3k emails/mo | Week 10 |
| **Sentry** (Errors) | https://sentry.io | Yes | Week 13 |
| **PostHog** (Analytics) | https://posthog.com | Yes | Week 13 |

Sign up with GitHub for all of them.

---

## 11. Verify Everything

Run this in Ghostty:

```bash
brew --version && node --version && npm --version && git --version && gh auth status && claude --version && vercel --version && zed --version
```

Every line should show a version or confirmation. If something fails, go back to that step.

---

## Quick Smoke Test

Let's make sure the full pipeline works — creating a project, pushing to GitHub, and deploying:

```bash
mkdir ~/test-setup && cd ~/test-setup
echo '<h1>It works</h1>' > index.html
git init && git add . && git commit -m "test"
gh repo create test-setup --public --source=. --push
```

If that worked, clean up:
```bash
cd ~ && rm -rf ~/test-setup && gh repo delete test-setup --yes
```

---

## You're Ready

Go to **[Week 1](./week-01/README.md)**.
