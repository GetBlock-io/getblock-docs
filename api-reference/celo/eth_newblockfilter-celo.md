---
description: >-
  Example code for the eth_newBlockFilter JSON-RPC method. Complete guide on how
  to use eth_newBlockFilter JSON-RPC in GetBlock Web3 documentation.
---

# eth\_newBlockFilter - Celo

This method creates a filter to notify when new blocks arrive on the Celo network. With Celo's \~1-second block times, this filter updates rapidly.

{% hint style="info" %}
This method creates a filter and returns a filter ID. It takes no parameters.
{% endhint %}

## Parameters

* None

## Request Example

{% tabs %}
{% tab title="cURL" %}
{% code title="cURL" %}
```bash
curl -X POST https://go.getblock.io/<ACCESS-TOKEN>/ \
-H "Content-Type: application/json" \
-d '{
  "jsonrpc": "2.0",
  "id": "getblock.io",
  "method": "eth_newBlockFilter",
  "params": []
}'
```
{% endcode %}
{% endtab %}

{% tab title="JavaScript (Axios)" %}
{% code title="request.js" %}
```javascript
const axios = require('axios');

const url = 'https://go.getblock.io/<ACCESS-TOKEN>/';

const payload = {
  jsonrpc: '2.0',
  id: 'getblock.io',
  method: 'eth_newBlockFilter',
  params: []
};

axios.post(url, payload, {
  headers: { 'Content-Type': 'application/json' }
})
.then(response => console.log(response.data))
.catch(error => console.error(error));
```
{% endcode %}
{% endtab %}

{% tab title="Python" %}
{% code title="request.py" %}
```python
import requests
import json

url = "https://go.getblock.io/<ACCESS-TOKEN>/"

payload = {
    "jsonrpc": "2.0",
    "id": "getblock.io",
    "method": "eth_newBlockFilter",
    "params": []
}

response = requests.post(url, headers={"Content-Type": "application/json"}, json=payload)
print(response.json())
```
{% endcode %}
{% endtab %}
{% endtabs %}

## Response Example

{% code title="response.json" %}
```json
{
  "jsonrpc": "2.0",
  "id": "getblock.io",
  "result": "0x1"
}
```
{% endcode %}

## Response Definition

| Field  | Type   | Description |
| ------ | ------ | ----------- |
| result | string | Filter ID   |

## Use Cases

* Monitor new block production
* Track network activity
* Trigger actions on new blocks
* Build block explorers

## Error Handling

| Error Code | Description    |
| ---------- | -------------- |
| -32603     | Internal error |

## SDK Integration

{% tabs %}
{% tab title="Ethers.js" %}
{% code title="ethers.js" %}
```javascript
const { ethers } = require('ethers');

const provider = new ethers.JsonRpcProvider('https://go.getblock.io/<ACCESS-TOKEN>/');

provider.on('block', (blockNumber) => {
  console.log('New block:', blockNumber);
});
```
{% endcode %}
{% endtab %}
{% endtabs %}
