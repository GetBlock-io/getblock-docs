---
title: Setup the project directory python
---

{% stepper %}
{% step %}
## Setup the project directory

```bash
mkdir celo-api-quickstart
cd celo-api-quickstart
```
{% endstep %}

{% step %}
## Create and activate a virtual environment

```bash
python -m venv venv
source venv/bin/activate

# On Windows, use venv\Scripts\activate
```
{% endstep %}

{% step %}
## Install requests

```bash
pip install requests
```
{% endstep %}

{% step %}
## Create script

Create a file called `main.py` with the following content:

{% code title="main.py" %}
```python
import requests
import json

url = "https://go.getblock.io/<ACCESS-TOKEN>/"

payload = json.dumps({
    "jsonrpc": "2.0",
    "method": "eth_blockNumber",
    "params": [],
    "id": "getblock.io"
})

headers = {
    'Content-Type': 'application/json'
}

response = requests.post(url, headers=headers, data=payload)
print(response.text)
```
{% endcode %}

Replace `<ACCESS-TOKEN>` with your actual access token from GetBlock.
{% endstep %}

{% step %}
## Run the script

```bash
python main.py
```
{% endstep %}
{% endstepper %}
