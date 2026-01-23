---
description: >-
  Example code for the eth_uninstallFilter JSON-RPC method. Complete guide on
  how to use eth_uninstallFilter JSON-RPC in GetBlock Web3 documentation.
---

# eth\_uninstallFilter - Celo

This method removes a filter created by `eth_newFilter` or `eth_newBlockFilter` on the Celo network. Call this method when a filter is no longer needed.

## Parameters

| Parameter | Type   | Required | Description         |
| --------- | ------ | -------- | ------------------- |
| filterId  | string | Yes      | Filter ID to remove |

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
  "method": "eth_uninstallFilter",
  "params": ["0x1"]
}'
```
{% endcode %}
{% endtab %}

{% tab title="Axios" %}
{% code title="JavaScript (Axios)" %}
```javascript
const axios = require('axios');

const url = 'https://go.getblock.io/<ACCESS-TOKEN>/';

const payload = {
  jsonrpc: '2.0',
  id: 'getblock.io',
  method: 'eth_uninstallFilter',
  params: ['0x1']
};

axios.post(url, payload, {
  headers: { 'Content-Type': 'application/json' }
})
.then(response => console.log(response.data))
.catch(error => console.error(error));
```
{% endcode %}
{% endtab %}

{% tab title="Request" %}
{% code title="Python" %}
```python
import requests
import json

url = "https://go.getblock.io/<ACCESS-TOKEN>/"

payload = {
    "jsonrpc": "2.0",
    "id": "getblock.io",
    "method": "eth_uninstallFilter",
    "params": ["0x1"]
}

response = requests.post(url, headers={"Content-Type": "application/json"}, json=payload)
print(response.json())
```
{% endcode %}
{% endtab %}
{% endtabs %}

## Response Example

{% code title="Response" %}
```json
{
  "jsonrpc": "2.0",
  "id": "getblock.io",
  "result": true
}
```
{% endcode %}

## Response Definition

| Field  | Type    | Description                  |
| ------ | ------- | ---------------------------- |
| result | boolean | `true` if filter was removed |

## Use Cases:

* Clean up unused filters
* Free up server resources
* Proper filter lifecycle management

## Error handling

| Error Code | Description       |
| ---------- | ----------------- |
| -32602     | Invalid filter ID |
| -32603     | Internal error    |
