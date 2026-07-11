# LLM Integration Playbook (Jupyter)

This repository contains Jupyter notebooks used to test and compare LLM integrations across multiple providers and models.

## Goals & Learning

Set out to learn:
- SDK setup and authentication across OpenAI, Gemini, Perplexity, and Anthropic
- API style differences (Responses API vs Chat Completions) and provider-specific endpoint/model compatibility
- LangChain abstractions for provider-agnostic workflows
- AI-assisted development workflow including AGENTS.md and SKILLS.md (with GitHub Copilot, Claude Code)

What came out of it:
- Diagnosed a [bug in Dimagi Open Chat Studio (OCS)](https://github.com/dimagi/open-chat-studio/issues/2962): Perplexity's Sonar models use chat-completions style, but its Agent API uses an OpenAI-compatible `/v1` base URL. OCS's LLM abstraction layer didn't account for that split. The investigation also clarified how [OCS's LLM service abstraction layer](https://github.com/dimagi/open-chat-studio/blob/main/apps/service_providers/llm_service/README.md) is built.

## Notebooks

| Notebook | Purpose | Key techniques |
|---|---|---|
| `OpenAI.ipynb` | OpenAI Responses API and Chat Completions | API key loading (`python-dotenv`); `instructions` vs role-based input array; legacy Chat Completions reference |
| `Gemini.ipynb` | Google Gemini SDK and LangChain Google integration | Direct `google-genai` usage (`genai.Client`); content generation & thinking config; `ChatGoogleGenerativeAI` |
| `Prerplexity.ipynb` | Perplexity Sonar, Search, and Agent API behavior | Sonar calls via `requests`/`perplexityai`; Search API; Agent API via OpenAI-SDK-compatible base URL |
| `Prerplexity-OCS-bug-repro.ipynb` | Reproduces the OCS bug above | Intentional endpoint mismatches showing 404/400 behavior |
| `Claude.ipynb` | Anthropic API behavior | API key validation & error handling; message creation; token counting/usage; tool use via `@beta_tool` |
| `LangChain-openai.ipynb` | LangChain wrappers over OpenAI: prompt templates, tool binding, structured output | `ChatOpenAI` invocation patterns; Responses API tool binding (web search); prompt templates (`langchain-core`); chain composition; structured output via Pydantic |
| `LangChain-perplexity.ipynb` | LangChain wrapper over Perplexity | `ChatPerplexity` basic invocation; note on `use_responses_api` incompatibility |

`requirements.txt` holds the Python dependencies shared across all notebooks.

## Prerequisites

- Python 3.12+ (tested on Linux).
- VS Code with Jupyter extension.
- API keys for LLM providers you want to test.

## Environment Setup

```bash
python -m venv .venv
source .venv/bin/activate
pip install -U pip
pip install -r requirements.txt
```

Create a `.env` file in the repository root:

```dotenv
OPENAI_API_KEY=
GOOGLE_API_KEY=
PERPLEXITY_API_KEY=
PPLX_API_KEY=
ANTHROPIC_API_KEY=
```

`Prerplexity.ipynb` uses `PERPLEXITY_API_KEY`; `LangChain-perplexity.ipynb` expects `PPLX_API_KEY` — you can set both to the same value.

In VS Code, open a notebook and select the Python kernel from `.venv`.

## AI assisted development and CI

**Optional — pre-commit hooks:** `pip install pre-commit detect-secrets && pre-commit install` sets up notebook output stripping, cell lint/format checks, and secret detection on commit (config in `.pre-commit-config.yaml`).

**Dependency maintenance:** use the `audit-dependencies` skill ([.agents/skills/audit-dependencies/SKILL.md](.agents/skills/audit-dependencies/SKILL.md)) to check for outdated/vulnerable packages in `requirements.txt` and apply safe bumps.

**Code Review with Claude** This project has a GitHub Actions workflow (`claude-code-review.yml`) that uses Claude Code for reviwing. Mention `@claude` in a PR comment for a code review


## Troubleshooting

- `ValueError ... API_KEY environment variable not set`:
	- Ensure `.env` exists and keys are populated.
	- Confirm the notebook kernel uses the same `.venv` where `python-dotenv` is installed.
- `401 Unauthorized` :
	- Verify key validity and account credits.
- `404 Not Found` (Perplexity):
	- Check endpoint matches the API style (Sonar chat completions vs Agent API).
- Run by Line and Debugging features for Python notebooks requires ipykernel v6 or greater to be installed in the notebook's kernel
	- `pip install -U ipykernel`

## References

See `AGENTS.md` for links to LLM API documentation
