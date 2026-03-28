# CHANGELOG — EcosystemEcologyLab/lab-principles

All notable changes to lab principles and standards are recorded here.
This file is the record of how the lab's practices have evolved over time.

Format: [Version] — [Date] — [Brief description]

---

## v1.0 — March 2026

Initial release. Developed during setup of the fluxnet-annual-2026 repository.

**Files added:**
- `SCIENCE_PRINCIPLES.md` v1.0 — universal scientific conscience
- `SCIENCE_PRINCIPLES_PIPELINES.md` v1.0 — data pipeline projects
- `SCIENCE_PRINCIPLES_TEXT_ANALYSIS.md` v1.0 — text analysis projects
- `CODESPACE_SETUP.md` v1.0 — GitHub Codespace setup guide for R projects
- `templates/devcontainer.json` — minimal working devcontainer configuration
- `templates/gitignore` — standard lab .gitignore
- `templates/env.example` — standard environment variable template
- `templates/CLAUDE_template.md` — CLAUDE.md skeleton for new projects
- `prompts/new_project_init.md` — standard first Claude Code prompt

**Key decisions recorded:**
- devcontainer base image: ghcr.io/rocker-org/devcontainer/tidyverse:4.4
  (not :4.4.2 — patch tag does not exist for devcontainer images)
- Claude Code authentication: CLAUDE_CODE_OAUTH_TOKEN via GitHub Codespace
  Secrets, granted per-repository
- Confidence vocabulary (HIGH/MEDIUM/LOW/UNKNOWN) is project-type-specific,
  not universal — lives in SCIENCE_PRINCIPLES_TEXT_ANALYSIS.md only
- Output metadata and exclusion logging conventions are pipeline-specific —
  live in SCIENCE_PRINCIPLES_PIPELINES.md
