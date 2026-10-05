
## Project Overview

A side project to test and compare LLM integrations across multiple providers and models using Jupyter Notebooks, python 3.12+

The engineer of this repo is using it to learn about LLM provider API style differences, LangChain and exploring the features of each LLM. Features include: RAG, Indexing, vector stores, embedding models, document loaders, LLM function calling, LLM parameters

## Repo layout

- `Notebooks/`: all notebooks, one per provider or topic (see the table in `README.md`)
- `requirements.txt`: Python dependencies shared across all notebooks, so new notebooks do not add their own
- `.env`: API keys, in the repository root, never committed

## Setup

```bash
python -m venv .venv
source .venv/bin/activate
pip install -U pip
pip install -r requirements.txt
```

In VS Code, open a notebook and select the Python kernel from `.venv`.

## Environment variables

Create a `.env` file in the repository root:

```dotenv
OPENAI_API_KEY=
GOOGLE_API_KEY=
PERPLEXITY_API_KEY=
PPLX_API_KEY=
ANTHROPIC_API_KEY=
```

`Perplexity.ipynb` uses `PERPLEXITY_API_KEY`; `LangChain-perplexity.ipynb` expects `PPLX_API_KEY`. Both can be set to the same value.

## Provider API constraints

- Perplexity Sonar models use chat-completions style; the Perplexity Agent API uses an OpenAI-compatible `/v1` base URL. Mixing them up gives 404/400 errors.
- `ChatPerplexity` does not support `use_responses_api`.
- OpenAI Responses API `file_search` accepts at most 2 vector store ids per call; a 3rd gives a 400 error.

## Pre-commit hooks

Optional: `pip install pre-commit detect-secrets && pre-commit install`. On commit, hooks strip notebook outputs, check cell lint/format, and scan for secrets (config in `.pre-commit-config.yaml`).

## Troubleshooting

- `ValueError ... API_KEY environment variable not set`:
	- Ensure `.env` exists and keys are populated.
	- Confirm the notebook kernel uses the same `.venv` where `python-dotenv` is installed.
- `401 Unauthorized`: verify key validity and account credits.
- `404 Not Found` (Perplexity): check the endpoint matches the API style (Sonar chat completions vs Agent API).
- Run by Line and Debugging for Python notebooks need ipykernel v6 or greater in the notebook's kernel: `pip install -U ipykernel`

## Style

- Top comments in a Jupyter Notebook should not be in the code, but in the markdown
- Functional inline comments stay in the code block

## Agent skills

### Issue tracker

GitHub Issues on `lisa-tarbo/LLM-integration-play` via the `gh` CLI.

### Dependency Audit

SKILL.md file in `.agents/skills/audit-dependencies`. Use it to check for outdated or vulnerable packages in `requirements.txt` and apply safe bumps; it writes an audit document and commits the bumped `requirements.txt`.

### Git rebase

`git-rebase` skill, based on the Dimagi skill and adapted for this repo.

### Code review

The GitHub Actions workflow `claude-code-review.yml` runs Claude Code reviews. Mention `@claude` in a PR comment to request one.

## Boundaries

- **Always**
  - When writing code suggest the smallest cheapest model to use to save
  - When writing code suggest mode parameters that are low cost. For example temperature = low, effort = low

- **Never**
  - Commit secrets, credentials, or tokens.
  - Use destructive git operations unless explicitly requested.

## References

- OpenAI API docs: https://developers.openai.com/api/docs
- Google Gemini API docs: https://ai.google.dev/gemini-api/docs/libraries
- Anthropic Claude: https://platform.claude.com/docs/en/cli-sdks-libraries/sdks/python
- LangChain core docs: https://reference.langchain.com/python/langchain-core
- LangChain integrations:
	- OpenAI: https://docs.langchain.com/oss/python/integrations/chat/openai
	- Google: https://docs.langchain.com/oss/python/integrations/chat/google_generative_ai

### Perplexity references

- Perplexity: https://docs.langchain.com/oss/python/integrations/chat/perplexity
- Reference: https://docs.perplexity.ai/docs/resources/faq#to-what-extent-is-the-api-openai-compatible
