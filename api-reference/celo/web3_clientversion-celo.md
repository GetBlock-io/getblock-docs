---
description: >-
  Example code for the web3_clientVersion JSON-RPC method. Complete guide on how
  to use web3_clientVersion JSON-RPC in GetBlock Web3 documentation.
---

# web3\_clientVersion - Celo

This method returns the current Celo client version string. Useful for debugging and compatibility checks.

## Parameters

* None

## Request Examples

{% tabs %}
{% tab title="cURL" %}
{% code title="curl" %}
```bash
curl -X POST https://go.getblock.io/<ACCESS-TOKEN>/ \
-H "Content-Type: application/json" \
-d '{
  "jsonrpc": "2.0",
  "id": "getblock.io",
  "method": "web3_clientVersion",
  "params": []
}'
```
{% endcode %}
{% endtab %}

{% tab title="JavaScript (Axios)" %}
{% code title="index.js" %}
```javascript
const axios = require('axios');

const url = 'https://go.getblock.io/<ACCESS-TOKEN>/';

const payload = {
  jsonrpc: '2.0',
  id: 'getblock.io',
  method: 'web3_clientVersion',
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
{% code title="example.py" %}
```python
import requests
import json

url = "https://go.getblock.io/<ACCESS-TOKEN>/"

payload = {
    "jsonrpc": "2.0",
    "id": "getblock.io",
    "method": "web3_clientVersion",
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
    "result": "Geth/v0.1.0-untagged-b3cca990-20250813/linux-amd64/go1.24.6"
}
```
{% endcode %}

## Response Definition

| Field  | Type   | Description           |
| ------ | ------ | --------------------- |
| result | string | Client version string |

## Use Cases

* Verify node version
* Check for compatibility
* Debug node issues
* Monitor infrastructure

## Error Handling

| Error Code | Description    |
| ---------- | -------------- |
| -32603     | Internal error |
