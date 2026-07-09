
# AGENTS.md

This file provides guidance to AI coding agents when working with code in this repository.

## Project Overview

A side project to test and compare LLM integrations across multiple providers and models using Jupyter Notebooks, python 3.12+
The engineer of this repo is using it to learn about LLM provider API style differences, LangChain and exploring the features of each LLM. Features include: RAG, Indexing, vector stores, embedding models, document loaders, LLM function calling, LLM parameters

## Agent skills

### Issue tracker

GitHub Issues on `lisa-tarbo/LLM-integration-play` via the `gh` CLI.

### Dependency Audit

SKILL.md file in `.agents/skills/audit-dependencies`

## Boundaries

- **Never**
  - Commit secrets, credentials, or tokens.
  - Use destructive git operations unless explicitly requested.

## References

- OpenAI API docs: https://developers.openai.com/api/docs
- Google Gemini API docs: https://ai.google.dev/gemini-api/docs/libraries
- LangChain core docs: https://reference.langchain.com/python/langchain-core
- LangChain integrations:
	- OpenAI: https://docs.langchain.com/oss/python/integrations/chat/openai
	- Google: https://docs.langchain.com/oss/python/integrations/chat/google_generative_ai

### Perplexity API references

- Perplexity: https://docs.langchain.com/oss/python/integrations/chat/perplexity
- Sonar API (chat completions style): `https://api.perplexity.ai/chat/completions`
- Agent API (OpenAI SDK compatible, responses style): `https://api.perplexity.ai/v1`
- Reference: https://docs.perplexity.ai/docs/resources/faq#to-what-extent-is-the-api-openai-compatible
