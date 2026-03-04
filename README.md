# Jupyter Notebooks for testing different LLMs


## The APIs for the LLMs

### Perplexity

Has 4 APIs
1) Agent API - fully compatible with OpenAI’s SDKs. (ie Responses API)
2) Search API -
3) Sonar API - provides retrieval-augmented generation (RAG) responses with real-time web search (model options: sonar, sonar-pro, sonar-deep-research, sonar-reasoning-pro )
4) Embeddings API -

OpenAI SDK Compatibility
- Use OpenAI SDKs with the Sonar API by changing the base URL to https://api.perplexity.ai/chat/completions
- Perplexity’s Sonar API is fully compatible with OpenAI’s Chat Completions interface but not the Agent API that uses Responseses API 
( ie Agent API https://api.perplexity.ai/v1 )

NOTE: For details on OpenAI compatibility, see https://docs.perplexity.ai/docs/resources/faq#to-what-extent-is-the-api-openai-compatible


### OpenAI APIs
Use the Responses API over the older Chat Completions API as suggested here: https://developers.openai.com/api/docs

### Google Gemini APIs
https://ai.google.dev/gemini-api/docs/libraries

## Setup Virtual Env

NOTE: Virtual environment setup tested on Python 3.13+ on Linux. 
Using Windows for Python v3.14 failed to install some langchain libraries as they were not built for windows

In terminal prepare the virtual environment
```console
cd C:\LisaData\Code\Python\LLM integration play

python -m venv .venv
.venv\Scripts\Activate.ps1 

#Note for Linux
source .venv/bin/activate
```

All python libs need to be installed in Virtual Environment

```console
pip install python-dotenv
pip install requests
pip install perplexityai
pip install openai
pip install google-genai
pip install langchain-core
```

##  Setup Jupyter Notebook
And then use VS Notebook: Select Notebook Kernel to select the virtual environment to test with Jupyter Notebook
And Visual Studio will ask you to install kernel libs in virtual env

## Setup API keys for each LLM

Suggest set them in a .env file

Or set them as below

```console
# For Linux
export OPENAI_API_KEY=""

# For Windows
setx OPENAI_API_KEY ""

# check environment variables 
printenv
echo $OPENAI_API_KEY
 ```

## LangChain
LangChain Core provides foundational abstractions and components for building LLM applications
Core components have the largest install base in the LLM ecosystem, and are used in production by many companies.
https://reference.langchain.com/python/langchain-core
