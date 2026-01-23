---
description: >-
  Example code for the net_peerCount JSON-RPC method. Complete guide on how to
  use net_peerCount JSON-RPC in GetBlock Web3 documentation.
---

# net\_peerCount - Celo

This method returns the number of peers currently connected to the Celo node. This is useful for monitoring network connectivity.

## Parameters

* None

## Request Example

{% tabs %}
{% tab title="cURL" %}
{% code title="curl" %}
```bash
curl -X POST https://go.getblock.io/<ACCESS-TOKEN>/ \
-H "Content-Type: application/json" \
-d '{
  "jsonrpc": "2.0",
  "id": "getblock.io",
  "method": "net_peerCount",
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
  method: 'net_peerCount',
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
    "method": "net_peerCount",
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
  "result": "0x19"
}
```
{% endcode %}

## Response Definition

| Field  | Type   | Description                     |
| ------ | ------ | ------------------------------- |
| result | string | Number of connected peers (hex) |

## Use Cases

* Monitor network connectivity
* Assess node health
* Debug connection issues

## Error Handling

| Error Code | Description    |
| ---------- | -------------- |
| -32603     | Internal error |
