# LLM Integration Playbook (Jupyter)

This repository contains Jupyter notebooks used to test and compare LLM integrations across multiple providers and models.

## Goals & Learning

Set out to learn:
- SDK setup and authentication across OpenAI, Gemini, Perplexity, and Anthropic
- API style differences (Responses API vs Chat Completions) and provider-specific endpoint/model compatibility
- LangChain abstractions for provider-agnostic workflows
- AI-assisted development workflow including AGENTS.md and SKILLS.md (with GitHub Copilot, Claude Code)

What came out of it:

1. Diagnosed a [bug in Dimagi Open Chat Studio (OCS)](https://github.com/dimagi/open-chat-studio/issues/2962): Perplexity's Sonar models use chat-completions style, but its Agent API uses an OpenAI-compatible `/v1` base URL. OCS's LLM abstraction layer didn't account for that split. The investigation also clarified how [OCS's LLM service abstraction layer](https://github.com/dimagi/open-chat-studio/blob/main/apps/service_providers/llm_service/README.md) is built.
2. Researched: OpenAI's Responses API file_search tool successfully searches across 2 vector stores in a single call but hard-caps at 2 — a 3rd vector_store_id throws a 400 "maximum of 2 vector stores allowed" error, confirming the undocumented [Dimagi OCS Remote Index limitation](https://github.com/dimagi/open-chat-studio/pull/3815).

## Notebooks

| Notebook | Purpose | Key techniques |
|---|---|---|
| [`OpenAI.ipynb`](Notebooks/OpenAI.ipynb) | OpenAI Responses API and Chat Completions | API key loading (`python-dotenv`); `instructions` vs role-based input array; legacy Chat Completions reference |
| [`OpenAI-remote-vector-store.ipynb`](Notebooks/OpenAI-remote-vector-store.ipynb) | Responses API `file_search` tool over remote vector stores | Multi-vector-store search calls; reproduces the 2-vector-store hard cap (400 error) behind finding #2 above |
| [`Gemini.ipynb`](Notebooks/Gemini.ipynb) | Google Gemini SDK and LangChain Google integration | Direct `google-genai` usage (`genai.Client`); content generation & thinking config; `ChatGoogleGenerativeAI` |
| [`Perplexity.ipynb`](Notebooks/Perplexity.ipynb) | Perplexity Sonar, Search, and Agent API behavior | Sonar calls via `requests`/`perplexityai`; Search API; Agent API via OpenAI-SDK-compatible base URL |
| [`Perplexity-OCS-bug-repro.ipynb`](Notebooks/Perplexity-OCS-bug-repro.ipynb) | Reproduces the OCS bug above | Intentional endpoint mismatches showing 404/400 behavior |
| [`Claude.ipynb`](Notebooks/Claude.ipynb) | Anthropic API behavior | API key validation & error handling; message creation; token counting/usage; tool use via `@beta_tool` |
| [`LangChain-openai.ipynb`](Notebooks/LangChain-openai.ipynb) | LangChain wrappers over OpenAI: prompt templates, tool binding, structured output | `ChatOpenAI` invocation patterns; Responses API tool binding (web search); prompt templates (`langchain-core`); chain composition; structured output via Pydantic |
| [`LangChain-perplexity.ipynb`](Notebooks/LangChain-perplexity.ipynb) | LangChain wrapper over Perplexity | `ChatPerplexity` basic invocation; note on `use_responses_api` incompatibility |

`requirements.txt` holds the Python dependencies shared across all notebooks.

## Prerequisites

- Python 3.12+ (tested on Linux).
- VS Code with Jupyter extension.
- API keys for LLM providers you want to test.

## Setup and environment

See [AGENTS.md](AGENTS.md) for:

- [Setup](AGENTS.md#setup): virtual environment and `pip install -r requirements.txt`
- [Environment variables](AGENTS.md#environment-variables): the `.env` file and the API key names each notebook expects
- [Pre-commit hooks](AGENTS.md#pre-commit-hooks) (optional)
- [Troubleshooting](AGENTS.md#troubleshooting)

## AI assisted development and CI

Skills (`audit-dependencies`, `git-rebase`) and the `@claude` PR review workflow are described in [AGENTS.md](AGENTS.md#agent-skills).

## References

See [AGENTS.md](AGENTS.md#references) for links to LLM API documentation
