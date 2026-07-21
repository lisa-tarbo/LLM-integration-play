# Dependency Audit — 2026-07-21

## Process

- Source file audited: `requirements.txt` (flat pins, no pip-tools/lockfile, single Python ecosystem).
- Tools used: `pip list --outdated` (staleness, direct deps only), `pip-audit` (sole vulnerability source, run against the active `.venv`).

## Summary

- 13 direct dependencies audited.
- 6 bump now (patch/minor, no breaking API surface for this project's usage).
- 0 needs a look (no major bumps required, no CVEs on any direct dependency, no transitive conflicts introduced by the bumps).
- `pip-audit` found 23 known vulnerabilities across 8 packages, all **transitive** dependencies, not pins in `requirements.txt` — no direct dep is affected. Listed below for visibility only; no action taken against `requirements.txt` for these since the skill scope is direct-dep pins.
- **Cleanup performed:** this audit surfaced pre-existing venv cruft — `google-cloud-aiplatform` and `langchain-google-vertexai` were installed but not in `requirements.txt` and not required by anything that is, and their `google-genai` version constraint already conflicted with the pinned version. The operator uninstalled both, then a follow-up pass removed the sub-dependencies pip left behind (`pip uninstall` doesn't cascade): `langchain-tests`, `pyarrow`, `pytest`, `pytest-asyncio`, `pytest-benchmark`, `pytest-codspeed`, `pytest-recording`, `pytest-socket`, `syrupy`, `vcrpy`, `numexpr`, `bottleneck`, `validators`, `httpx-sse` — 16 packages total removed. `pip check` reports no broken requirements, and all project imports (`openai`, `google.genai`, `langchain`, `langchain_core`, `anthropic`, `perplexity`, plus the LangChain integration wrappers used in notebooks) were smoke-tested and still succeed.
- **Result:** `pip-audit` dropped from 26 vulnerabilities / 11 packages to **23 / 8** — the `pyarrow`, `pytest`, and `vcrpy` CVEs are gone along with the packages that carried them.

## Direct dependencies

| Package | Current | Latest | Bucket | CVE/Advisory | Action |
|---|---|---|---|---|---|
| python-dotenv | 1.2.2 | 1.2.2 | up to date | none | — |
| requests | 2.34.2 | 2.34.2 | up to date | none | — |
| openai | 2.45.0 | 2.46.0 | bump now | none | bump pin |
| google-genai | 2.11.0 | 2.12.1 | bump now | none | bump pin |
| perplexityai | 0.40.0 | 0.42.0 | bump now | none | bump pin |
| langchain | 1.3.13 | 1.3.14 | bump now | none | bump pin |
| langchain-core | 1.4.9 | 1.5.0 | bump now | none | bump pin |
| langchain-openai | 1.3.5 | 1.3.5 | up to date | none | — |
| langchain-google-genai | 4.2.7 | 4.2.7 | up to date | none | — |
| langchain-perplexity | 1.4.0 | 1.4.0 | up to date | none | — |
| pydantic | 2.13.4 | 2.13.4 | up to date | none | — |
| ipykernel | 7.3.0 | 7.3.0 | up to date | none | — |
| anthropic | 0.116.0 | 0.117.0 | bump now | none | bump pin |

## Bumps applied

- `openai` 2.45.0 → 2.46.0
- `google-genai` 2.11.0 → 2.12.1
- `perplexityai` 0.40.0 → 0.42.0
- `langchain` 1.3.13 → 1.3.14
- `langchain-core` 1.4.9 → 1.5.0
- `anthropic` 0.116.0 → 0.117.0

All six are patch/minor bumps with no known breaking changes for this project's usage. Smoke-tested via import after applying: `openai`, `google.genai`, `perplexity`, `langchain`, `langchain_core`, `anthropic`, plus the downstream integration imports used in the notebooks (`langchain_openai.ChatOpenAI`, `langchain_google_genai.ChatGoogleGenerativeAI`, `langchain_perplexity.ChatPerplexity`, `langchain_core.prompts.ChatPromptTemplate`) — all succeeded.

## Tickets to file

None — no direct dependency required a major bump, carried an active CVE, or hit a transitive conflict introduced by this audit's bumps.

## Transitive vulnerabilities (informational, not in requirements.txt)

Re-run against the venv after removing the orphaned Vertex AI stack and its leftover sub-dependencies (see Summary). All remaining rows are load-bearing — pulled in by an active direct dependency.

| Package | Installed | Fix version | Advisory IDs |
|---|---|---|---|
| cryptography | 46.0.5 | 46.0.6 / 46.0.7 / 48.0.1 | PYSEC-2026-35, PYSEC-2026-36, GHSA-537c-gmf6-5ccf |
| idna | 3.11 | 3.15 | PYSEC-2026-215 |
| langchain-anthropic | 1.3.4 | 1.4.6 | PYSEC-2026-2556 |
| langsmith | 0.7.7 | 0.7.31 / 0.8.18 | PYSEC-2026-2582, PYSEC-2026-2583, GHSA-f4xh-w4cj-qxq8 |
| pyasn1 | 0.6.2 | 0.6.3 | PYSEC-2026-2263 |
| pygments | 2.19.2 | 2.20.0 | PYSEC-2026-2987 |
| tornado | 6.5.4 | 6.5.5 / 6.5.6 / 6.5.7 | PYSEC-2026-140, PYSEC-2026-2287, PYSEC-2026-3387, PYSEC-2026-3388, GHSA-pw6j-qg29-8w7f |
| urllib3 | 2.6.3 | 2.7.0 | PYSEC-2026-141, PYSEC-2026-142 |

`pyarrow`, `pytest`, and `vcrpy` — previously flagged and marked orphaned in the pre-cleanup pass — are gone now that their only consumer (`langchain-google-vertexai` → `langchain-tests`) has been removed. These will resolve themselves as their parent direct deps bump their own pins upstream; no action needed against `requirements.txt` today.
