# CHANGELOG — EcosystemEcologyLab/lab-principles

All notable changes to lab principles and standards are recorded here.
This file is the record of how the lab's practices have evolved over time.

Format: [Version] — [Date] — [Brief description]

---

## v2.0 — July 2026

Added the execution model to the universal principles and reworked the CLAUDE.md
template into a lean core plus opt-in modules.

**Files changed:**

- `SCIENCE_PRINCIPLES.md` v1.0 → v2.0 — added the execution model: the durable
  on-disk run log (written first, kept independent of version control, the record
  when the terminal is lost), and autonomous unattended execution with
  self-commit. Scoped that autonomy to execution and procedure only, with an
  explicit boundary that it never extends to scientific judgment (thresholds, QC
  cutoffs, and inclusion calls remain the scientist's per the human-authority
  pillar). Retired the prior in-session commit-approval expectation in favor of
  the run-log-plus-session-journal audit trail.
- `templates/CLAUDE_template.md` — replaced with a lean core plus opt-in modules.
  The core carries only project-specific operational content and points to
  `SCIENCE_PRINCIPLES.md` by version and commit rather than restating rules.
  Modules (manual overrides, two-layer reproducibility, table QA, drift-audit
  standing rule) are deletable and carry only project parameters.
- `templates/SESSION_LOG_template.md` — added. Seed session journal that
  demonstrates the entry header format.

**Key decisions recorded:**

- The SESSION_LOG (session journal) convention is operational as well as
  scientific, so it is defined in `CLAUDE.md` (the project file), not in
  `SCIENCE_PRINCIPLES.md`. The principles file references the session journal as
  part of the audit trail but does not define its format.
- Session journal entry headers use a full UTC datetime, `YYYY-MM-DD HH:MM UTC`,
  because there are often several sessions in a day. Entries are committed and
  pushed before handback; an uncommitted entry is treated as lost.
- Execution autonomy and scientific judgment are separated deliberately: an
  unattended task may decide how to proceed operationally but may not decide what
  is scientifically true.
- No rule is stated in two documents. `SCIENCE_PRINCIPLES.md` and the CLAUDE.md
  template reference each other rather than duplicating text.

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
