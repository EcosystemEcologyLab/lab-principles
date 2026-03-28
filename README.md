# EcosystemEcologyLab — Lab Principles

This repository is the single source of truth for how the Moore Lab
(EcosystemEcologyLab, University of Arizona) sets up research projects,
works with Claude Code, and maintains scientific standards across all work.

**Maintained by:** David J.P. Moore  
**Organisation:** EcosystemEcologyLab, University of Arizona  

---

## What this repository contains

| File / Directory | Purpose |
|---|---|
| `SCIENCE_PRINCIPLES.md` | Universal scientific conscience — applies to every project |
| `SCIENCE_PRINCIPLES_PIPELINES.md` | Additional rules for data pipeline projects |
| `SCIENCE_PRINCIPLES_TEXT_ANALYSIS.md` | Additional rules for text and document analysis projects |
| `CODESPACE_SETUP.md` | Step-by-step guide to setting up Claude Code in a GitHub Codespace |
| `templates/` | Ready-to-use template files for new projects |
| `prompts/new_project_init.md` | Standard first Claude Code prompt for new project initialisation |

---

## Who should follow these standards

All lab members working on projects that are:
- Published in peer-reviewed journals
- Submitted to government bodies or funding agencies
- Conducted in collaboration with external partners
- Supported by external funding

For exploratory or personal work, these standards are recommended but not
required. When in doubt, follow the standards — they exist to protect you
as much as the science.

---

## How to start a new project

1. Create a new repository under `EcosystemEcologyLab` on GitHub
2. Add `CLAUDE_CODE_OAUTH_TOKEN` access to the new repository
   (see `CODESPACE_SETUP.md` Part 1)
3. Copy `templates/devcontainer.json` to `.devcontainer/devcontainer.json`
4. Copy `templates/gitignore` to `.gitignore`
5. Copy `templates/env.example` to `.env.example`
6. Copy `templates/CLAUDE_template.md` to `CLAUDE.md` and fill in the
   project-specific sections
7. Copy the relevant principles files to the repository root:
   - Always copy `SCIENCE_PRINCIPLES.md`
   - Copy `SCIENCE_PRINCIPLES_PIPELINES.md` for data pipeline projects
   - Copy `SCIENCE_PRINCIPLES_TEXT_ANALYSIS.md` for text analysis projects
8. Record the commit hash of this repository in your `CLAUDE.md`
   (see `templates/CLAUDE_template.md` for the format)
9. Commit and push all files
10. Open the Codespace and follow `CODESPACE_SETUP.md`
11. Paste the contents of `prompts/new_project_init.md` into Claude Code

---

## How to update lab standards

When best practices change:

1. Open an issue in this repository describing what should change and why
2. Make the change in a branch and open a pull request
3. Increment the version number in the header of the affected file
4. Update the `CHANGELOG.md` with a brief description of what changed
5. Merge to main
6. Existing projects are NOT automatically updated — they retain the
   version of standards they were initialised with
7. For active projects, the PI will decide whether to adopt the update
   based on whether results have already been published

---

## Versioning

Each principles file carries a version number in its header. When you
copy a file into a project, record the version and the source commit hash
in that project's `CLAUDE.md`. This ensures that any published result can
be traced back to the exact version of standards that governed it.

---

## Questions

If you are a lab member with questions about these standards, talk to
David Moore before starting a new project. Do not improvise deviations
from these standards without discussion — the standards exist to protect
the reproducibility of the lab's published work.
