# SCIENCE_PRINCIPLES.md, version 2.0 additions

These are the additive sections that take SCIENCE_PRINCIPLES.md from v1.0 to v2.0.
They introduce the execution model (durable run log; autonomous, unattended
operation) that v1.0 does not yet contain. Nothing here overrides the four
pillars or the v1.0 conduct rules; it extends them. Insert the two sections
below, bump the header version to 2.0, and add the changelog note.

Note on placement: the SESSION_LOG (session journal) convention is deliberately
NOT defined here. It is operational as well as scientific and lives in each
project's CLAUDE.md. This file references the session journal as part of the
audit trail but does not define its format.

---

## Header change

Change the version line in the file header from `Version: 1.0` to `Version: 2.0`.

---

## New section: Execution model

Add this as a top-level section, after the four pillars and before or within the
conduct rules, whichever reads better in the current file.

### The durable run log

Any task that is not being watched live writes a run log to disk as its first
action, before it does anything else. It appends a line at each significant
step, and it writes a final outcome line on success or on failure.

The run log is a plain file on disk, kept independent of version control. This
is deliberate. The terminal on a remote machine can corrupt on a crash or a
disconnect, and any record that exists only in terminal output is then lost. The
on-disk run log, not the terminal, is the record. A run log written only to the
terminal has not been written.

The run log is regenerable working output, so a project normally gitignores it;
its persistence comes from being on disk during and after the run, not from
being committed. The session journal (defined per project in CLAUDE.md) is the
committed, human-readable counterpart; the two together are the audit trail for
autonomous work.

### Autonomous execution and commits

Tasks in this lab are often launched to run unattended, with no one watching.
Such a task runs from start to finish without pausing for interaction. It
resolves operational and procedural decisions using stated defaults, records
each such default in the run log, and commits and pushes its own outputs. The
run log and the session journal are the audit trail for what it did and why.

This retires any earlier expectation of in-session human approval before each
commit. Approval is not the control; the recorded audit trail is.

This autonomy is bounded, and the boundary is not negotiable. It covers
execution and procedure only. It never extends to scientific judgment.
Classification thresholds, QC cutoffs, inclusion and exclusion decisions, and
the reading of a surprising or anomalous result remain the scientist's, exactly
as the human-authority pillar requires. An unattended task that reaches a
scientific judgment call does not resolve it silently and does not substitute a
default for it. It records the outcome as UNKNOWN, or it stops and flags, and it
leaves the decision for a human. Verify a surprising result before committing
it.

In short: an unattended task may decide how to proceed operationally; it may not
decide what is scientifically true.

---

## Changelog note (add to CHANGELOG.md)

    ## v2.0 — <DATE>

    Added the execution model to SCIENCE_PRINCIPLES.md: the durable on-disk run
    log, and autonomous unattended execution with self-commit. Clarified that
    execution autonomy never extends to scientific judgment (the human-authority
    pillar governs thresholds, QC cutoffs, and inclusion calls regardless of
    whether a run is attended). Retired the prior in-session commit-approval
    expectation in favor of the run-log-plus-session-journal audit trail. The
    session-journal (SESSION_LOG) convention is defined per project in CLAUDE.md,
    not here.
