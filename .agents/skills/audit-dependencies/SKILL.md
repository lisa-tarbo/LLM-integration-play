---
name: audit-dependencies
description: Run a dependency audit for this project's plain requirements.txt (Python, no pip-tools, no JS). Produces report, applies safe bumps, emits Jira-ready ticket list for risky items. Use on demand or for periodic maintenance.
---

# Audit Dependencies

## Overview

Audit of this project's direct dependencies, pinned in `requirements.txt`, against PyPI and known-vulnerability data. Produces `docs/dependency-audit-YYYY-MM-DD.md`, applies safe bumps directly to `requirements.txt`, and emits a ticket list for risky ones.

This project has a single ecosystem: Python via a flat `requirements.txt` (no `.in`/pip-tools compile step, no JS/npm). Skip any tooling not present — there is no lockfile-compile workflow here.

## Step 0: Discover project structure

1. Confirm the venv (`.venv`) is active; if not, activate it per README.md.
2. Install current pins: `pip install -r requirements.txt`.
3. Snapshot direct deps from `requirements.txt` — this is both the direct-dep list and the pin file (no separate lockfile).

## Steps

1. **Check installed vs. latest**: `pip list --outdated` for the installed venv, cross-referenced against the names in `requirements.txt` (ignore transitive-only packages that aren't direct deps).

2. **Check for known vulnerabilities**: run `pip-audit` with no arguments, against the active venv — do not use `pip-audit -r requirements.txt`; that flag makes it build an isolated resolver venv via `ensurepip`, which fails in environments without `python3-venv` installed. This is the sole vulnerability source — no separate PyPI/OSV/endoflife.date queries needed for a project this size. Flag any direct dep (one listed in `requirements.txt`) that appears in the results.

3. **Classify each direct dep** into one of two buckets (see Classification below):
   - **Bump now** — patch/minor bump, or a major bump with no breaking API surface change for this project's usage (spot-check with `grep`/notebook read).
   - **Needs a look** — major version bump where the API surface likely changed, or a transitive conflict shows up (e.g. another installed package pins an incompatible range).

## Report and tickets

4. **Write report** to `docs/dependency-audit-<today>.md` with:
   - **Process section:** tools used (`pip list --outdated`, `pip-audit`), source file audited (`requirements.txt`).
   - Summary (dep counts per bucket)
   - Per-package table: current version, latest version, bucket, CVE/advisory IDs if any, action
   - "Bumps applied" list
   - "Tickets to file" list (needs-a-look items)

5. **Apply "bump now" changes** directly to `requirements.txt` (edit the `==` pin), then `pip install -r requirements.txt`.

   After applying, smoke-test by importing each bumped package in the venv (this project has no test suite — it's notebooks, not a package with pytest coverage). If an import fails or errors obviously, move that package to "needs a look" and revert its pin.

6. **Emit ticket list** for "needs a look" items to `docs/dependency-audit-<today>-tickets.md` using Jira-ready format (title, current→target, risk, references, and whether it's blocked by a transitive conflict).

7. **Commit the applied bumps** in a single commit covering the `requirements.txt` change. Do not commit the audit report or ticket list — leave those for the operator. Do not push or open a PR.

### Classification

- **Bump now:** patch/minor, or a major bump verified not to touch this project's usage.
- **Needs a look:** major bump with likely breaking changes, active CVE with no compatible fix short of a breaking upgrade, or a transitive dependency conflict (e.g. `pip install` reports an incompatible range).

## When to invoke

- On demand, or periodically as maintenance.
- After noticing a security advisory for one of the direct deps.

## References

- PyPI: https://pypi.org
- OSV vulnerability DB (used by `pip-audit`): https://osv.dev
