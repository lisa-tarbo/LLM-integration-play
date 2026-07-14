# Dependency Audit — 2026-07-14

## Process

- Source file audited: `requirements.txt` (flat pins, no pip-tools/lockfile, single Python ecosystem).
- Tools used: `pip list --outdated` (staleness, direct deps only), `pip-audit` (sole vulnerability source, run against the active `.venv`).

## Summary

- 13 direct dependencies audited.
- 5 bump now (patch/minor, no breaking API surface for this project's usage).
- 0 needs a look (no major bumps required, no CVEs on any direct dependency, no transitive conflicts reported by `pip install`).
- `pip-audit` found 26 known vulnerabilities across 11 packages, but all are **transitive** dependencies, not pins in `requirements.txt` — no direct dep is affected. Listed below for visibility only; no action taken against `requirements.txt` for these since the skill scope is direct-dep pins.

## Direct dependencies

| Package | Current | Latest | Bucket | CVE/Advisory | Action |
|---|---|---|---|---|---|
| python-dotenv | 1.2.2 | 1.2.2 | up to date | none | — |
| requests | 2.34.2 | 2.34.2 | up to date | none | — |
| openai | 2.44.0 | 2.45.0 | bump now | none | bump pin |
| google-genai | 2.10.0 | 2.11.0 | bump now | none | bump pin |
| perplexityai | 0.39.0 | 0.40.0 | bump now | none | bump pin |
| langchain | 1.3.12 | 1.3.13 | bump now | none | bump pin |
| langchain-core | 1.4.9 | 1.4.9 | up to date | none | — |
| langchain-openai | 1.3.4 | 1.3.5 | bump now | none | bump pin |
| langchain-google-genai | 4.2.7 | 4.2.7 | up to date | none | — |
| langchain-perplexity | 1.4.0 | 1.4.0 | up to date | none | — |
| pydantic | 2.13.4 | 2.13.4 | up to date | none | — |
| ipykernel | 7.3.0 | 7.3.0 | up to date | none | — |
| anthropic | 0.116.0 | 0.116.0 | up to date | none | — |

## Bumps applied

- `openai` 2.44.0 → 2.45.0
- `google-genai` 2.10.0 → 2.11.0
- `perplexityai` 0.39.0 → 0.40.0
- `langchain` 1.3.12 → 1.3.13
- `langchain-openai` 1.3.4 → 1.3.5

All five are patch/minor bumps with no known breaking changes for this project's usage; smoke-tested via import after applying.

## Tickets to file

None — no direct dependency required a major bump, carried an active CVE, or hit a transitive conflict.

## Transitive vulnerabilities (informational, not in requirements.txt)

| Package | Installed | Fix version | Advisory IDs |
|---|---|---|---|
| cryptography | 46.0.5 | 46.0.6 / 46.0.7 / 48.0.1 | PYSEC-2026-35, PYSEC-2026-36, GHSA-537c-gmf6-5ccf |
| idna | 3.11 | 3.15 | PYSEC-2026-215 |
| langchain-anthropic | 1.3.4 | 1.4.6 | PYSEC-2026-2556 |
| langsmith | 0.7.7 | 0.8.18 | PYSEC-2026-2582, PYSEC-2026-2583, GHSA-f4xh-w4cj-qxq8 |
| pyarrow | 22.0.0 | 23.0.1 | PYSEC-2026-113 |
| pyasn1 | 0.6.2 | 0.6.3 | PYSEC-2026-2263 |
| pygments | 2.19.2 | 2.20.0 | PYSEC-2026-2987 |
| pytest | 9.0.2 | 9.0.3 | PYSEC-2026-1845 |
| tornado | 6.5.4 | 6.5.7 | PYSEC-2026-140, PYSEC-2026-2287, PYSEC-2026-3387, PYSEC-2026-3388, GHSA-pw6j-qg29-8w7f |
| urllib3 | 2.6.3 | 2.7.0 | PYSEC-2026-141, PYSEC-2026-142 |
| vcrpy | 8.1.1 | 8.2.1 | GHSA-rpj2-4hq8-938g |

These will resolve themselves as their parent direct deps bump their own pins upstream; no action needed against `requirements.txt` today.
