# LLM Integration Playbook (Jupyter)

This repository contains four Jupyter notebooks used to test and compare LLM integrations across OpenAI, Google Gemini, Perplexity, and LangChain.

## Repository Contents

- `Open AI.ipynb`: OpenAI Responses API and Chat Completions examples.
- `Gemini.ipynb`: Google Gemini SDK and LangChain Google integration examples.
- `Prerplexity.ipynb`: Perplexity Sonar, Search, and Agent API behavior experiments.
- `LangChain.ipynb`: LangChain model wrappers, prompt templates, tool usage, and structured outputs.
- `requirements.txt`: Python dependencies used across the notebooks.

## Goals

- Validate SDK setup and authentication for each provider.
- Compare API styles (Responses API vs Chat Completions).
- Test provider-specific endpoint/model compatibility.
- Explore LangChain abstractions for reusable, provider-agnostic workflows.

## Prerequisites

- Python 3.13+ (tested on Linux).
- VS Code with Jupyter extension.
- API keys for providers you want to test.

Notes:
- Some packages may fail on certain Windows/Python combinations due to binary wheel availability.
- Run all installs inside a virtual environment.

## Setup

### 1) Create and activate a virtual environment

Linux/macOS:

```bash
python -m venv .venv
source .venv/bin/activate
```

### 2) Install dependencies

```bash
pip install -U pip
pip install -r requirements.txt
```

### 3) Configure API keys

Create a `.env` file in the repository root:

```dotenv
OPENAI_API_KEY=
GOOGLE_API_KEY=
PERPLEXITY_API_KEY=
PPLX_API_KEY=
```

Important:
- `Prerplexity.ipynb` uses `PERPLEXITY_API_KEY`.
- `LangChain.ipynb` Perplexity cells expect `PPLX_API_KEY`.
- You can set both to the same key value.

### 4) Select kernel in VS Code

- Open a notebook.
- Choose the Python kernel from `.venv`.
- If prompted, install notebook dependencies for that kernel.

## Notebook Guide

### 1) Open AI.ipynb

What it covers:
- API key loading with `python-dotenv`.
- OpenAI Responses API examples.
- Instruction style comparisons (`instructions` vs role-based input array).
- Legacy Chat Completions example for reference.

Required env vars:
- `OPENAI_API_KEY`

### 2) Gemini.ipynb

What it covers:
- Direct `google-genai` usage (`genai.Client`).
- Content generation and thinking configuration.
- LangChain integration via `ChatGoogleGenerativeAI`.

Required env vars:
- `GOOGLE_API_KEY`

### 3) Prerplexity.ipynb

What it covers:
- Sonar API calls using `requests` and `perplexityai` library.
- Search API usage.
- Agent API usage through OpenAI SDK-compatible base URL.
- Intentional endpoint mismatch examples showing 404/400 behaviors.

Required env vars:
- `PERPLEXITY_API_KEY`

### 4) LangChain.ipynb

What it covers:
- `ChatOpenAI` and model invocation patterns.
- Responses API tool binding in LangChain.
- Prompt templates with `langchain-core`.
- `ChatPerplexity` usage.
- Simple chain composition and structured output extraction with Pydantic.

Required env vars:
- `OPENAI_API_KEY`
- `GOOGLE_API_KEY` (for Google integration cells)
- `PPLX_API_KEY` (for Perplexity integration cells)

## Perplexity API Notes

- Sonar API endpoint (chat completions style):
	- `https://api.perplexity.ai/chat/completions`
- Agent API endpoint (OpenAI SDK compatible responses style):
	- `https://api.perplexity.ai/v1`

Compatibility reminder:
- Sonar models are used with chat completions style calls.
- Not all model/endpoint combinations are valid; some notebook cells intentionally demonstrate invalid pairings for learning.

Reference:
- https://docs.perplexity.ai/docs/resources/faq#to-what-extent-is-the-api-openai-compatible

## Troubleshooting

- `ValueError ... API_KEY environment variable not set`:
	- Ensure `.env` exists and keys are populated.
	- Confirm the notebook kernel uses the same `.venv` where `python-dotenv` is installed.
- `401 Unauthorized` (Perplexity):
	- Verify key validity and account credits.
- `404 Not Found` (Perplexity):
	- Check endpoint matches the API style (Sonar chat completions vs Agent API).
- Package install issues:
	- Upgrade `pip` first and retry in a fresh virtual environment.

## References

- OpenAI API docs: https://developers.openai.com/api/docs
- Google Gemini API docs: https://ai.google.dev/gemini-api/docs/libraries
- LangChain core docs: https://reference.langchain.com/python/langchain-core
- LangChain integrations:
	- OpenAI: https://docs.langchain.com/oss/python/integrations/chat/openai
	- Google: https://docs.langchain.com/oss/python/integrations/chat/google_generative_ai
	- Perplexity: https://docs.langchain.com/oss/python/integrations/chat/perplexity
