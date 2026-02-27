# Creating test Jupyter Noteboos that calls LLM API for testing different LLMs


## API using

1) Perplexity API https://api.perplexity.ai/chat/completions
2) open AI https://developers.openai.com/api/docs


## Setup Virtual Env and test API in Jupyter Notebook

NOTE: Make a virtual environment for python ON LINUX V12.13 
failed to do on V Env on windows for Linux v3.14 t


In terminal
```console
cd C:\LisaData\Code\Python\LLM integration play

python -m venv .venv
.venv\Scripts\Activate.ps1 

python.exe -m pip install --upgrade pip 

pip install python-dotenv

pip install perplexityai
pip install openai
pip install requests

```

And then use VS Notebook: Select Notebook Kernel to select the virtual environment to test with Jupyter Notebook

And Visual Studio will ask you to install kernel libs in virtual env

