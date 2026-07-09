---
name: audit-dependencies
description: Run a full dependency audit for a project — covers Python (pip-tools) and jupyter notebooks if present. Produces report, applies safe bumps, emits Jira-ready ticket list for risky/EoL items. Use when running quarterly maintenance or on demand.
---

# Audit Dependencies

## Overview

End-to-end audit of a project's direct dependencies against PyPI, npm, and `endoflife.date`. Produces `docs/dependency-audit-YYYY-MM-DD.md`, applies category A+B bumps to the lockfiles, and emits a ticket list for category C+D items.

Covers Python — pip-tools (`*.in` → `*.txt` workflow). Detect which Python toolchain the project uses and run that one; do not mix the two. If a project only has one ecosystem, skip the steps for the other.

## Step 0: Discover project structure

Before starting, orient yourself:

1. **Python:** First determine the toolchain.
   - **pip-tools:** Otherwise find `.in` files: `find . -name "*.in" -path "*/requirements*" | grep -v node_modules`, and their corresponding lockfiles (`*.txt`).
2. **Major framework:** Read the Python lockfile to determine the current Django (or other framework) version — this sets the "framework floor" for Step 6.
3. Active the virtual environment for the project (see README.md for instructions). If the project has a `requirements.txt` file, install it in the venv.

## Steps

### Python

1. **Snapshot current pins** from the lockfile(s) discovered in Step 0 (`*.txt` for pip-tools).

2. **Compute available upgrades:**
   - **pip-tools:** `pip-compile --upgrade --dry-run` on each `.in` file.

3. **For each direct dep** (from the `.in` files), fetch latest version from PyPI:
   ```bash
   python -c "import json,urllib.request; print(json.load(urllib.request.urlopen('https://pypi.org/pypi/<pkg>/json'))['info']['version'])"
   ```

4. **EoL check** for framework-grade components via `https://endoflife.date/api/<product>.json`. At minimum check: `python`, `django` (if used), `postgres` (if used). Flag anything currently in use that is past EoL or within 6 months of EoL.

5. **Classify each Python dep** using the A/B/C/D scheme (see Classification below).

6. **Check framework floor.** For each B/A candidate, verify whether the target version requires a newer version of the major framework (Django, etc.) than currently running. If yes and the framework upgrade is itself deferred to a ticket, demote the package to C and link it to that ticket.

### Report and tickets

11. **Write report** to `docs/dependency-audit-<today>.md` with:
    - **Process section:** classification criteria (A/B/C/D definitions), tools used (pip-compile or uv lock — whichever the project uses, PyPI API, npm audit, npm outdated, endoflife.date, git grep, pip-audit), and source files audited.
    - Summary (dep counts per category, split by ecosystem if both present)
    - EoL findings table
    - Per-package table (Python and JS sections if both present): current version, latest version, category, action, changelog link
    - "Bumps to apply" list (A + B)
    - "Tickets to file" list (C + D)

12. **Apply Python A+B bumps**, one package at a time using the project's toolchain:
    - **pip-tools:** `pip-compile --upgrade-package <pkg>`.

    After each bump, run the test suite and `pre-commit run -a`. If anything breaks, demote to C and revert.

    Commit structure:
    - One bulk commit for all A-class bumps
    - One bulk commit for all B-class bumps that required no code changes
    - One commit per B-class bump that required minor code changes (so it can be reverted in isolation)
    - JS bumps in their own commit(s), separate from Python bumps

13. **Emit ticket list** for C+D items to `docs/dependency-audit-<today>-tickets.md` using Jira-ready format (title, current→target, EoL date, risk, references). Roll framework-floor-blocked packages into the framework upgrade ticket.

14. **Commit the applied bumps** following the structure in step 12. Do not commit the audit report or ticket list — leave that to the operator. Do not push or open a PR.

### Classification

Used for both Python and JS deps:

- **A — Patch/minor:** non-breaking version bump.
- **B — Low-risk major:** major bump where the changelog has no breaking changes affecting this project's import/usage surface (verify with `git grep` for Python; check import/usage patterns for JS).
- **C — Risky major:** breaking changes affect the code.
- **D — EoL / security:** upstream support ended, within 6 months of EoL, or active CVE.

## When to invoke

- Quarterly maintenance (see `docs/dependency-maintenance.md` if it exists in this project).
- After a Dependabot security alert if a broader review is wanted.
- Before planning a major framework upgrade.

## References

- pip-tools docs: https://pip-tools.readthedocs.io
- endoflife.date API: https://endoflife.date/api/<product>.json
- OSV vulnerability DB (used by pip-audit): https://osv.dev
- npm audit docs: https://docs.npmjs.com/cli/commands/npm-audit
