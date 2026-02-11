---
title: Setup project
---

{% stepper %}
{% step %}
## Setup project

Create and initialize a new project:

```bash
mkdir celo-api-quickstart
cd celo-api-quickstart
npm init --yes
```
{% endstep %}

{% step %}
## Install Axios

```bash
npm install axios
```
{% endstep %}

{% step %}
## Create file

Create a new file named `index.js`. This is where you will make your first call.
{% endstep %}

{% step %}
## Set ES module type

Set the ES module `"type": "module"` in your `package.json`.
{% endstep %}

{% step %}
## Add code

Add the following code to `index.js`:

{% code title="index.js" %}
```javascript
import axios from 'axios';

const data = JSON.stringify({
    "jsonrpc": "2.0",
    "method": "eth_blockNumber",
    "params": [],
    "id": "getblock.io"
});

const config = {
    method: 'post',
    url: 'https://go.getblock.io/<ACCESS-TOKEN>/',
    headers: {
        'Content-Type': 'application/json'
    },
    data: data
};

axios(config)
    .then(response => console.log(JSON.stringify(response.data)))
    .catch(error => console.log(error));
```
{% endcode %}

Replace `<ACCESS-TOKEN>` with your actual access token from GetBlock.
{% endstep %}

{% step %}
## Run the script

```bash
node index.js
```

Expected output (example):

```json
{
    "jsonrpc": "2.0",
    "id": "getblock.io",
    "result": "0x1d9f2a8"
}
```
{% endstep %}
{% endstepper %}
