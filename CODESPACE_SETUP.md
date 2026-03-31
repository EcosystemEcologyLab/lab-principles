# CODESPACE_SETUP.md — Claude Code in GitHub Codespaces

**Version:** 1.2
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
Codespace Secret. This token is stored at the account level.

### 1.2 Token Expiry and Regeneration

The `CLAUDE_CODE_OAUTH_TOKEN` expires after approximately 90 days, or immediately
if you run `claude /logout` on **any device**.

> ⚠️ **Never run `claude /logout` in a Codespace during troubleshooting.**
> It revokes the token globally and breaks authentication in all Codespaces immediately.

**To regenerate the token when it expires or is revoked:**

1. On your local Mac terminal (not in a Codespace):
```bash
claude setup-token
```
2. Copy the printed token — it starts with `sk-ant-oat01-` and has no spaces
3. Go to `https://github.com/settings/codespaces`
4. Find `CLAUDE_CODE_OAUTH_TOKEN` → click **Update** → paste the new token → save
5. Delete any existing Codespace and create a fresh one

**Signs the token needs regenerating:**
- `CLAUDE_CODE_OAUTH_TOKEN` is present in the environment but Claude Code returns "Invalid API key"
- Authentication worked previously but fails after running `claude /logout`

### 1.3 Granting Token Access to a New Repository

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
- Do not add the `sshd` feature — it interferes with Codespace Secret injection,
  preventing `CLAUDE_CODE_OAUTH_TOKEN` from being available in the environment

> ⚠️ **Any change to devcontainer.json requires deleting and recreating the
> Codespace to take effect. Test changes carefully — a broken devcontainer
> cannot be fixed by rebuilding, only by deleting and recreating.**

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

### Step 0 — Verify secret injection (do this first)

Before installing anything, confirm the OAuth token was injected:

```bash
echo "OAUTH token present: $([ -n "$CLAUDE_CODE_OAUTH_TOKEN" ] && echo YES || echo NO)"
```

**If YES** — proceed to Step 1.
**If NO** — stop. Check that `CLAUDE_CODE_OAUTH_TOKEN` is granted to this
repository at `https://github.com/settings/codespaces`. Delete and recreate
the Codespace. Do not proceed until this prints YES.

### Step 1 — Fix renv lock if needed

Run this first — it is harmless if the lock does not exist:

```bash
rm -f ~/.cache/R/renv/library/fluxnet-annual-2026-0f379c84/linux-ubuntu-noble/R-4.4/x86_64-pc-linux-gnu/00LOCK-renv
```

Confirm Node.js is not yet installed:
```bash
node --version 2>/dev/null || echo "not installed"
```

### Step 2 — Install Node.js (~1 minute)

```bash
curl -fsSL https://deb.nodesource.com/setup_20.x | sudo -E bash - && sudo apt-get install -y nodejs
```

Confirm:
```bash
node --version
```

### Step 3 — Install Claude Code (~30 seconds)

```bash
sudo npm install -g @anthropic-ai/claude-code
```

Confirm:
```bash
claude --version
```

### Step 4 — Set up authentication

Save the token, unset both variables, then set only `ANTHROPIC_API_KEY`:

```bash
SAVED_TOKEN=$CLAUDE_CODE_OAUTH_TOKEN
unset CLAUDE_CODE_OAUTH_TOKEN
unset ANTHROPIC_API_KEY
export ANTHROPIC_API_KEY=$SAVED_TOKEN
```

Confirm the token is set correctly:
```bash
echo $ANTHROPIC_API_KEY | head -c 20
```

It should print `sk-ant-oat01-` followed by more characters. If it prints
nothing, the token was not injected — go back to Step 0.

### Step 5 — Start Claude Code

```bash
claude
```

### Step 6 — Navigate the prompts

| Prompt | Select |
|---|---|
| Do you want to use this API key? | **1. Yes** |
| Use recommended terminal settings? | **1. Yes** |
| Trust this folder? | **1. Yes** |

### Step 7 — Verify

Type `hi` and press Enter. If Claude responds, setup is complete.

---

## Part 5: Quick Reference Card

```bash
# 0. Verify secret injection — must print YES before proceeding
echo "OAUTH token present: $([ -n "$CLAUDE_CODE_OAUTH_TOKEN" ] && echo YES || echo NO)"

# 1. Fix renv lock (harmless if not needed)
rm -f ~/.cache/R/renv/library/fluxnet-annual-2026-0f379c84/linux-ubuntu-noble/R-4.4/x86_64-pc-linux-gnu/00LOCK-renv

# 2. Install Node.js (~1 min)
curl -fsSL https://deb.nodesource.com/setup_20.x | sudo -E bash - && sudo apt-get install -y nodejs

# 3. Install Claude Code (~30 sec)
sudo npm install -g @anthropic-ai/claude-code

# 4. Set up authentication
SAVED_TOKEN=$CLAUDE_CODE_OAUTH_TOKEN
unset CLAUDE_CODE_OAUTH_TOKEN
unset ANTHROPIC_API_KEY
export ANTHROPIC_API_KEY=$SAVED_TOKEN

# 5. Start Claude Code
claude
# → Prompt 1: Select 1. Yes (use this API key)
# → Prompt 2: Select 1. Yes (recommended terminal settings)
# → Prompt 3: Select 1. Yes (trust this folder)
```

---

## Part 6: Troubleshooting

| Symptom | Cause | Fix |
|---|---|---|
| Recovery mode on Codespace open | Image tag does not exist (e.g. `:4.4.2`) | Delete Codespace, fix tag to `:4.4`, push, create fresh |
| `CLAUDE_CODE_OAUTH_TOKEN` empty (Step 0 prints NO) | Token not granted to this repository | Add repo at `github.com/settings/codespaces`, delete and recreate |
| `CLAUDE_CODE_OAUTH_TOKEN` present but "Invalid API key" | Token expired or revoked by `claude /logout` | Regenerate token on local Mac with `claude setup-token`, update GitHub secret, recreate Codespace |
| Auth conflict warning (both token and API key set) | Both variables present simultaneously | Use the Step 4 sequence which saves, unsets both, then sets only `ANTHROPIC_API_KEY` |
| `claude: command not found` | Claude Code not installed | Run Steps 2 and 3 |
| `npm: command not found` | Node.js not installed | Run Step 2 first |
| `npm EACCES permission denied` | Missing sudo | Use `sudo npm install -g @anthropic-ai/claude-code` |
| renv lock error on Codespace open | Two R processes collided during startup | Run the `rm -f` command in Step 1 then `Rscript -e "renv::restore()"` |
| sshd feature breaks secret injection | sshd interferes with Codespace Secret injection | Remove sshd from devcontainer.json, commit, delete and recreate Codespace |

---

## Part 7: Important Warnings

> ⚠️ **Never run `claude /logout` in a Codespace.** It revokes the token globally.

> ⚠️ **Any devcontainer.json change requires a full delete and recreate** — not just a rebuild. Test changes conservatively. When in doubt, ask whether the change could affect secret injection or the R environment before making it.

> ⚠️ **Do not add devcontainer features without verifying they are compatible with the rocker image and Codespace Secret injection.** The `sshd` feature, Python feature, and `RENV_PATHS_CACHE` are all known to cause problems.

---

## Part 8: Before Creating a New Codespace — Checklist

- [ ] `CLAUDE_CODE_OAUTH_TOKEN` granted to the new repository
- [ ] Token is not expired (last regenerated within 90 days, no `claude /logout` run since)
- [ ] `.devcontainer/devcontainer.json` committed using `tidyverse:4.4` with no problematic features
- [ ] `CLAUDE.md` committed to the repository root
- [ ] `SCIENCE_PRINCIPLES.md` (and relevant specialised files) committed
- [ ] Any required Codespace Secrets added to the repository
