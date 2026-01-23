---
description: >-
  Example code for the eth_accounts JSON-RPC method. Complete guide on how to
  use eth_accounts JSON-RPC in GetBlock Web3 documentation.
---

# eth\_accounts - Celo

This method returns a list of addresses owned by the client. For public RPC endpoints like GetBlock, this typically returns an empty array since no accounts are stored server-side.

## Parameters

* None

## Request examples

{% tabs %}
{% tab title="cURL" %}
{% code title="cURL" %}
```bash
curl -X POST https://go.getblock.io/<ACCESS-TOKEN>/ \
-H "Content-Type: application/json" \
-d '{
  "jsonrpc": "2.0",
  "id": "getblock.io",
  "method": "eth_accounts",
  "params": []
}'
```
{% endcode %}
{% endtab %}

{% tab title="JavaScript (Axios)" %}
{% code title="JavaScript (Axios)" %}
```javascript
const axios = require('axios');

const url = 'https://go.getblock.io/<ACCESS-TOKEN>/';

const payload = {
  jsonrpc: '2.0',
  id: 'getblock.io',
  method: 'eth_accounts',
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
{% code title="Python" %}
```python
import requests
import json

url = "https://go.getblock.io/<ACCESS-TOKEN>/"

payload = {
    "jsonrpc": "2.0",
    "id": "getblock.io",
    "method": "eth_accounts",
    "params": []
}

response = requests.post(url, headers={"Content-Type": "application/json"}, json=payload)
print(response.json())
```
{% endcode %}
{% endtab %}
{% endtabs %}

## Response example

{% code title="Response JSON" %}
```json
{
  "jsonrpc": "2.0",
  "id": "getblock.io",
  "result": []
}
```
{% endcode %}

## Response Definition

| Field  | Type  | Description                                      |
| ------ | ----- | ------------------------------------------------ |
| result | array | List of addresses (usually empty for public RPC) |

## Use cases

* Check for locally stored accounts
* Verify RPC configuration
* Wallet compatibility checks

## Error handling

| Error Code | Description    |
| ---------- | -------------- |
| -32603     | Internal error |
