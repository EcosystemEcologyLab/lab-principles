# CODESPACE_SETUP.md — Claude Code in GitHub Codespaces

**Version:** 1.0  
**Applies to:** All EcosystemEcologyLab R projects  
**Tested on:** fluxnet-annual-2026, March 2026  

---

## Time required

- One-time prerequisite setup: ~5 minutes (done once, applies to all future Codespaces)
- Per-Codespace setup: ~8 minutes (3–5 min build + 3 min Claude Code install and auth)

---

## Part 1: One-Time Prerequisites

### 1.1 The CLAUDE_CODE_OAUTH_TOKEN Secret

Claude Code authenticates using an OAuth token stored as a personal GitHub
Codespace Secret. This token is stored at the account level and does not need
to be regenerated for each project.

### 1.2 Granting Token Access to a New Repository

Every time you create a new repository where you want to use Claude Code,
do this **before** opening a Codespace:

1. Go to `https://github.com/settings/codespaces`
2. Find the secret named `CLAUDE_CODE_OAUTH_TOKEN`
3. Click on it to edit
4. Under "Repository access", add the new repository to the list
5. Save

> **If you skip this step**, Claude Code will fall into a browser
> authentication loop. The only fix is to add the repository to the token's
> access list and then delete and recreate the Codespace.

---

## Part 2: The devcontainer.json

Every new R project needs a `.devcontainer/devcontainer.json` committed
before the Codespace is opened. Use the template at
`templates/devcontainer.json` in this repository.

**Critical rules:**
- Use `tidyverse:4.4` not `tidyverse:4.4.2` — the patch version tag does
  not exist for devcontainer images and causes an immediate build failure
- Keep the file minimal — do not add `onCreateCommand` or `postCreateCommand`
- Do not add the Python devcontainer feature — Python is managed internally
  by reticulate
- Do not add `RENV_PATHS_CACHE` — the path does not exist and breaks renv

To create this file on your local Mac:

```bash
cd /path/to/your/repository
mkdir .devcontainer
cp /path/to/lab-principles/templates/devcontainer.json .devcontainer/devcontainer.json
# Edit the "name" field to match your repository name
git add .devcontainer/devcontainer.json
git commit -m "Add devcontainer configuration"
git push
```

---

## Part 3: Opening the Codespace

1. Go to the repository on GitHub
2. Click the green **Code** button → **Codespaces** tab → **Create codespace on main**
3. Wait 3–5 minutes for the build to complete

> **If you see a recovery mode error**: delete the Codespace immediately —
> do not rebuild. Go to `https://github.com/codespaces`, find the Codespace,
> click `...` → **Delete**. Fix the `devcontainer.json`, push the fix, and
> create a fresh Codespace. Rebuilding a broken Codespace takes as long as a
> fresh build and leaves the environment in an inconsistent state.

---

## Part 4: Installing and Authenticating Claude Code

These steps are required every time you create a new Codespace.
They are not persisted between Codespace deletions.

### Step 1 — Install Node.js (~1 minute)

```bash
curl -fsSL https://deb.nodesource.com/setup_20.x | sudo -E bash - && sudo apt-get install -y nodejs
```

### Step 2 — Install Claude Code (~30 seconds)

```bash
sudo npm install -g @anthropic-ai/claude-code
```

### Step 3 — Fix the auth conflict

The rocker image sets `ANTHROPIC_API_KEY` which conflicts with the OAuth
token. Unset it before starting Claude:

```bash
unset ANTHROPIC_API_KEY
```

### Step 4 — Start Claude Code

```bash
claude
```

### Step 5 — Navigate the prompts

| Prompt | Select |
|---|---|
| Select login method / API key detected | **2. No** |
| Use recommended terminal settings? | **1. Yes** |
| Trust this folder? | **1. Yes** |

> **Why select No?** The `CLAUDE_CODE_OAUTH_TOKEN` is injected automatically
> from Codespace Secrets. Selecting No tells Claude Code to use it instead of
> the now-unset `ANTHROPIC_API_KEY`. Selecting Yes causes an auth conflict
> and Claude Code will fail to run.

### Step 6 — Verify

You should see the Claude Code welcome screen. Type `hi` and press Enter.
If Claude responds, setup is complete.

---

## Part 5: Quick Reference Card

```bash
# Run these commands every time you open a new Codespace, in order:

# 1. Install Node.js (~1 min)
curl -fsSL https://deb.nodesource.com/setup_20.x | sudo -E bash - && sudo apt-get install -y nodejs

# 2. Install Claude Code (~30 sec)
sudo npm install -g @anthropic-ai/claude-code

# 3. Fix auth conflict
unset ANTHROPIC_API_KEY

# 4. Start Claude Code
claude
# → Prompt 1: Select No (ignore detected API key)
# → Prompt 2: Select Yes (recommended terminal settings)
# → Prompt 3: Select Yes (trust this folder)
```

---

## Part 6: Troubleshooting

| Symptom | Cause | Fix |
|---|---|---|
| Recovery mode on Codespace open | Image tag does not exist (e.g. `:4.4.2`) | Delete Codespace, fix tag to `:4.4`, push, create fresh |
| Browser auth loop | `CLAUDE_CODE_OAUTH_TOKEN` not granted to this repository | Add repo at `github.com/settings/codespaces`, delete and recreate |
| Auth conflict warning | Both `ANTHROPIC_API_KEY` and `CLAUDE_CODE_OAUTH_TOKEN` set | `unset ANTHROPIC_API_KEY` then restart `claude` |
| `claude: command not found` | Claude Code not installed | Run the Node.js and npm install steps |
| `npm: command not found` | Node.js not installed | Run the curl Node.js install command first |
| `npm EACCES permission denied` | Missing sudo | Use `sudo npm install -g @anthropic-ai/claude-code` |
| Invalid API key error | Auth conflict not resolved | `Ctrl+C`, `unset ANTHROPIC_API_KEY`, restart `claude`, select No |

---

## Part 7: Before Creating a New Codespace — Checklist

- [ ] `CLAUDE_CODE_OAUTH_TOKEN` granted to the new repository
- [ ] `.devcontainer/devcontainer.json` committed using `tidyverse:4.4`
- [ ] `CLAUDE.md` committed to the repository root
- [ ] `SCIENCE_PRINCIPLES.md` (and relevant specialised files) committed
- [ ] Any required Codespace Secrets added to the repository
