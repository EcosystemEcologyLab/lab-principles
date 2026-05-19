# Getting Started with Claude Code

## Pre-session preparation for the 3-hour workshop

---

## What you're walking into

Claude Code is an AI assistant that runs *inside* your project — in a terminal, alongside your files, with the ability to read, write, and execute code. It is different from the chatbot version of Claude that you might have used in a browser. Where the chatbot is a conversation about code, Claude Code is a collaborator that actually changes files, runs scripts, commits to git, and reports back.

We use it because it removes the friction of copying code between a chat window and your editor. Claude can see the whole project, understand the context, and propose changes that work in place. You stay in control — Claude pauses for confirmation before doing anything destructive — but the speed of iteration is dramatically higher than copy-paste-from-a-chatbot workflows.

By the end of the session you will have:

- A working Claude Code setup in a GitHub Codespace, attached to a research repository of your own
- A working understanding of when to use Claude Code versus other tools
- A starter prompt and workflow you can adapt to your own work

This document covers what you need to do **before** the session so we can spend the three hours doing real work rather than untangling account setup.

---

## Section 1 — Before the session: account setup

This is the longest part of the prep. Plan to do it in advance — not the morning of the session — because some steps require email verification or have variable wait times.

### What you will need

- A laptop with a recent web browser (Chrome, Firefox, Safari, or Edge — all fine)
- A working internet connection
- About 30–90 minutes of total prep time, depending on which accounts you already have

### Step 1 — GitHub account

If you already have a GitHub account, skip to Step 2.

If you do not:

1. Go to https://github.com/signup
2. Use an email address you check regularly. If your institution provides one, use that — many universities have free GitHub Pro through GitHub Education.
3. Choose a username you would not mind appearing on commits or pull requests. It is visible publicly.
4. Verify your email.

Time estimate: 10–15 minutes. If you hit a verification puzzle that fails repeatedly, try a different browser.

### Step 2 — Repository access

Send your GitHub username to Dave before the session. Dave will add you to the lab's working repository so you have somewhere to practice.

Once Dave adds you, you will receive an email invitation. Accept it before the session.

### Step 3 — Anthropic account and Claude Code OAuth token

This is the most important step and the one most likely to cause problems on the day of the session if left until the last minute. Please do this at least 24 hours in advance.

You need a Claude account with API access (specifically, the ability to generate an OAuth token for Claude Code). The simplest path:

1. Go to https://claude.ai and create an account (or sign in with one you already have).
2. Go to https://console.anthropic.com — this is the developer console, separate from the consumer chat interface. Sign in.
3. From the console, you will set up a Claude Code OAuth token. The exact UI for this changes occasionally; the simplest way to generate the right kind of token is to install Claude Code locally on your laptop (instructions below) and run `claude login` once. This opens a browser window, you authenticate, and the token is saved.

To install Claude Code locally for the one-time login:

- On Mac or Linux: open a terminal and run `npm install -g @anthropic-ai/claude-code`. If you do not have `npm`, install Node.js first from https://nodejs.org.
- On Windows: install Node.js from https://nodejs.org, then open PowerShell and run `npm install -g @anthropic-ai/claude-code`.

Then run `claude login`, follow the browser flow, and you should see a confirmation that you are logged in.

After this, copy the contents of your OAuth token. On Mac and Linux, the token is stored in `~/.config/claude-code/` or accessible via environment variable `CLAUDE_CODE_OAUTH_TOKEN`. The simplest way to retrieve it is to run, in a terminal:

```
echo $CLAUDE_CODE_OAUTH_TOKEN
```

If that prints a long string starting with `sk-ant-oat01-`, that is your token. Save it somewhere you can find it again (a password manager is ideal).

Time estimate: 20–45 minutes, depending on whether you already have Node.js installed and whether you hit any browser issues during the OAuth flow.

If you get stuck on this step, contact Dave **before the session**, not during it.

### Step 4 — Add the OAuth token to GitHub Codespaces

Once you have the token:

1. Go to https://github.com/settings/codespaces
2. Scroll to "Codespaces secrets" and click "New secret"
3. Name: `CLAUDE_CODE_OAUTH_TOKEN`
4. Value: paste the token from Step 3
5. Repository access: pick "Selected repositories" and add the lab repository Dave gave you access to. You can add others later as needed.
6. Save.

This makes the token available to Claude Code automatically when you launch a Codespace from that repository.

Time estimate: 5 minutes.

### Step 5 — Confirm everything works

You do not need to test this before the session — we will do it together — but if you want to confirm your setup is complete, you can:

1. Go to the lab repository on GitHub
2. Click the green **Code** button → **Codespaces** → **Create codespace on main**
3. Wait 3–5 minutes for the Codespace to build
4. When the terminal opens, type `echo $CLAUDE_CODE_OAUTH_TOKEN` — if you see your token printed, you are good.

If you see nothing or get an error, double-check Step 4. The most common failure is the secret not being granted to the specific repository.

---

## Section 2 — Before the session: 15-minute orientation

These are quick reads that will make the session more productive. None are required, but if you have time, the first two are worth it.

### Recommended

**The lab principles repository** at https://github.com/EcosystemEcologyLab/lab-principles. Skim `SCIENCE_PRINCIPLES.md` and `CODESPACE_SETUP.md`. These are the standards we work to and the technical setup we assume across all lab projects. You do not need to memorize anything; just know they exist and roughly what they cover.

**Anthropic's quick intro to Claude Code** at https://docs.claude.com. Read the overview page only — twenty minutes maximum. The key concepts to take away: Claude Code lives in a terminal, you talk to it in plain language, it can read and write files in your project, and it can run commands.

### Optional, if you have time

The fluxnet-annual-2026 repository (https://github.com/EcosystemEcologyLab/fluxnet-annual-2026) is a real, in-progress lab project that uses Claude Code daily. The recent commit history and issue threads show what real Claude-assisted work looks like. You will not be asked to understand the science; the value is seeing the *workflow*.

---

## Section 3 — On the day of the session

### What to bring

A laptop with a working browser. That is it. You do not need to install anything else locally. All the work happens in a Codespace running on GitHub's servers.

### What to expect

The session has three parts.

**First hour:** demo. Dave will set up a fresh Codespace, launch Claude Code, and walk through a real task end-to-end. You can follow along in your own browser if you want, but you do not have to.

**Second hour:** your turn. Bring a small piece of work you would like to tackle — ideally something with lightweight data (a CSV, a small API you can pull from) and a clear goal. If you do not have something, Dave will provide a starter project.

**Third hour:** patterns and pitfalls. We will look at what worked, what did not, and walk through the patterns that make Claude Code productive (and the patterns that waste time).

### What NOT to do

Please do not pre-launch a Codespace before the session starts. Codespaces have monthly free-tier limits and starting them early eats into that budget. We will start them together.

---

## Section 4 — A few principles we use

These are short on purpose. The full version is in the lab principles repository. For now, just the working norms:

**Claude works with you, not for you.** You are still the scientist, the engineer, and the decision-maker. Claude is a tool that drafts, suggests, and executes — but you read, review, and approve. Treat its output the way you would treat output from a smart but very junior collaborator.

**Always review before committing.** Claude can commit to git on your behalf. That does not mean it should without you reading the diff first. Get into the habit of asking Claude to show you what changed before pushing.

**Stop and confirm before destructive actions.** Deletes, force-pushes, mass file modifications — Claude will pause and ask you. Always read the question carefully and never reflexively say yes.

**The Codespace is sandboxed, but credentials and pushes are real.** A Codespace is an isolated environment, so mistakes inside it do not break your laptop. But it is connected to your real GitHub account, and pushes go to real repositories. Your OAuth token, your AmeriFlux credentials, anything else stored as a Codespaces secret — those are real and need the same care as any other credential.

**Attribution matters.** When Claude writes substantial code that ends up in a paper or a public repository, we say so. The lab norm is to mention it in commit messages, methods sections, or acknowledgments as appropriate. We are not hiding the tool; we are using it openly.

---

## Section 5 — Where to go next

After the session:

- **Lab principles repository** — `EcosystemEcologyLab/lab-principles`. The canonical source for how we work.
- **Anthropic documentation** — `docs.claude.com`. The reference for Claude Code features.
- **Anthropic prompting guide** — `docs.claude.com/en/docs/build-with-claude/prompt-engineering/overview`. How to write prompts that get good results.
- **Office hours** — Dave will hold a follow-up office hour two weeks after the session. Bring questions, problems, or things you want to show off.

---

## Appendix — Glossary

A short list of terms you will hear in the session.

**Codespace.** A development environment that runs on GitHub's servers and opens in your browser. It includes a terminal, a code editor, and a copy of your repository. Think of it as "a Linux computer you can access from any laptop, configured for your project."

**Devcontainer.** A configuration file in your repository that tells GitHub how to build a Codespace. The lab uses a standard one. You will not need to edit it.

**OAuth token.** A long string that authenticates you to a service without using a password. Yours grants Claude Code permission to act on your behalf. Treat it like a password.

**Repository (or "repo").** A project tracked in git. A folder of code with version history. The lab has several; you will start by working in one Dave gives you access to.

**Commit.** A saved snapshot of changes in git. Each commit has a message describing what changed. Claude can write commits on your behalf.

**Pull request (or "PR").** A proposal to merge changes from one branch into another. Used for code review. You probably will not open one in this session, but you will hear the term.

**Prompt.** What you type to Claude. Can be one sentence or several paragraphs. The quality of the prompt usually determines the quality of the result.

**Diff.** The set of differences between two versions of a file. When Claude proposes changes, it shows you a diff. Reading diffs carefully is one of the most valuable habits to develop.

---

*Questions before the session? Email Dave.*
