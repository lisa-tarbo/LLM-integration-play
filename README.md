# Jupyter Notebooks for testing different LLMs


## The APIs for the LLMs

### Perplexity

Has 4 APIs
1) Agent API - fully compatible with OpenAI’s SDKs. (POST Responses)
2) Search API
3) Sonar API - provides web-grounded AI responses (options: sonar, sonar-pro, sonar-deep-research, sonar-reasoning-pro )
4) Embaddings API

OpenAI SDK Compatibility
-Use OpenAI SDKs with the Sonar API by changing the base URL to https://api.perplexity.ai/chat/completions
- Perplexity’s Sonar API is fully compatible with OpenAI’s Chat Completions interface (MUST use this interface!!)
OR
-You can use your existing OpenAI client libraries with the Agent API by simply changing the base URL 
Agent API https://api.perplexity.ai/v1

NOTE: https://docs.perplexity.ai/docs/resources/faq#to-what-extent-is-the-api-openai-compatible


### open AI 
Use the Responses API over the older Chat Completions API

https://developers.openai.com/api/docs


## Setup Virtual Env and test API in Jupyter Notebook

NOTE: Make a virtual environment for python ON LINUX V12.13 
failed to do on V Env on windows for Linux v3.14 t

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

pip install perplexityai
pip install openai
pip install requests
pip install langchain-core
pip install -U langchain-openai
```

And then use VS Notebook: Select Notebook Kernel to select the virtual environment to test with Jupyter Notebook

And Visual Studio will ask you to install kernel libs in virtual env

## Setup API keys for each LLM

in .env file

```console
For Linux
export OPENAI_API_KEY=""

For Windows
setx OPENAI_API_KEY ""

printenv
echo $OPENAI_API_KEY
 ```

## LangChain
LangChain Core contains the base abstractions that power the LangChain ecosystem.
Core components have the largest install base in the LLM ecosystem, and are used in production by many companies.
https://reference.langchain.com/python/langchain-core