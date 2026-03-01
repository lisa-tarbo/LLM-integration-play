# Creating test Jupyter Noteboos that calls LLM API for testing different LLMs


## AI using different APIs for different LLMs

### Perplexity

Has 4 APIs
1) Agent API - fully compatible with OpenAI’s SDKs. 
2) Search API
3) Sonar API - provides web-grounded AI responses (options: sonar, sonar-pro, sonar-deep-research, sonar-reasoning-pro )
4) Embaddings API

Perplexity SDKs for Python and Typescript to access the Perplexity APIs with type safety and async support.

You can use your existing OpenAI client libraries with the Agent API by simply changing the base URL 
Agent API https://api.perplexity.ai/v1

NOTE: https://docs.perplexity.ai/docs/resources/faq#to-what-extent-is-the-api-openai-compatible


### open AI 
Use the Responses API over the older Chat Completions API

https://developers.openai.com/api/docs


## Setup Virtual Env and test API in Jupyter Notebook

NOTE: Make a virtual environment for python ON LINUX V12.13 
failed to do on V Env on windows for Linux v3.14 t

In terminal prepare the virtual environment - Windows
```console
cd C:\LisaData\Code\Python\LLM integration play

python -m venv .venv
.venv\Scripts\Activate.ps1 

```

In terminal prepare the virtual environment - Linux
```console
cd C:\LisaData\Code\Python\LLM integration play

python -m venv .venv
source .venv/bin/activate

```

All python libs need to be installed in Virtual Environment

```console
python.exe -m pip install --upgrade pip 

pip install python-dotenv

pip install perplexityai
pip install openai
pip install requests

```

And then use VS Notebook: Select Notebook Kernel to select the virtual environment to test with Jupyter Notebook

And Visual Studio will ask you to install kernel libs in virtual env

## Setup API keys for each LLM

See actual values in OneNote

```console
For Linux
export OPENAI_API_KEY=""

For Windows
setx OPENAI_API_KEY ""

 echo $OPENAI_API_KEY
 ```

