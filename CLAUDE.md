# CLAUDE.md — lab-principles

## Purpose

This repository is the single source of truth for lab standards,
templates, and Claude Code setup guides for the Moore Lab
(EcosystemEcologyLab, University of Arizona). It is not an analysis
pipeline. It produces no data outputs and runs no R scripts.

**PI:** David J.P. Moore, University of Arizona
**Repository:** https://github.com/EcosystemEcologyLab/lab-principles

---

## What this repository contains

- SCIENCE_PRINCIPLES.md — universal scientific standards for all projects
- SCIENCE_PRINCIPLES_PIPELINES.md — additional rules for pipeline projects
- SCIENCE_PRINCIPLES_TEXT_ANALYSIS.md — additional rules for text analysis
- templates/ — template files copied into new projects at initialisation
- prompts/ — standard Claude Code prompts for new project setup
- CODESPACE_SETUP.md — Codespace setup guide (being supplemented with
  local Mac mini setup documentation)

---

## Hard Rules

### 1. This is a standards repository, not an analysis repository
Do not run R scripts, install packages, or produce data outputs here.

### 2. Version numbers must be maintained
Every principles file carries a version number in its header. When editing
any principles file, increment the version number and update CHANGELOG.md.

### 3. Never modify templates without PI approval
Templates are copied into active projects at initialisation. Changing a
template does not update existing projects. Any template change must be
discussed with the PI first.

### 4. Git operations require explicit approval
Never commit, push, or merge without showing the proposed commit message
and changed files and waiting for PI confirmation.

---

## Autonomy

Apply standard local machine caution throughout. Ask before making any
changes to principles files, templates, or prompts. Reading and
summarising files is always permitted without confirmation.

---

## Template Drift and Periodic Review

Project-specific CLAUDE.md files and supporting .md files evolve during
active work and may contain innovations, conventions, or standards that
should be ported back to lab-principles as universal templates.

### Known drift to resolve
- Fsoil_aridlands contains a figure style guide (figure_style_defaults.md)
  that should be reviewed for incorporation into lab-principles templates
- CODESPACE_SETUP.md needs updating to reflect local Mac mini workflow
- new_project_init.md prompt needs updating for Mac mini and Claude
  Desktop rather than Codespace-only workflow

### Periodic review process
At the start of each semester, or when a project reaches a major
milestone (submission, revision, data lock), the PI should run a
cross-project review:

1. Read all .md files across active project repositories
2. Identify conventions or standards that appear in more than one project
   or that would benefit all future projects
3. Propose additions or revisions to templates/ or principles files
4. Open an issue in lab-principles for each proposed change
5. Update CHANGELOG.md when changes are merged

When working in lab-principles, Claude Code may be asked to assist with
this review by reading CLAUDE.md files across the active repositories
in ~/Research/R/ and summarising project-specific innovations that are
candidates for promotion to universal standards.

---

## Active Repositories to Monitor

- ~/Research/R/fluxnet-annual-2026
- ~/Research/R/IPCC_NatGHG
- ~/Research/R/Fsoil_aridlands
- ~/Research/R/FluxCourseForecast
- Student project repositories (locations to be added)
