---
description: >-
  Example code for the eth_getFilterChanges JSON-RPC method. Complete guide on
  how to use eth_getFilterChanges JSON-RPC in GetBlock Web3 documentation.
---

# eth\_getFilterChanges - Celo

The method returns an array of logs/block hashes that have occurred since the last poll for the given filter on the Celo network.

## Parameters

| Parameter | Type   | Required | Description                             |
| --------- | ------ | -------- | --------------------------------------- |
| filterId  | string | Yes      | Filter ID from newFilter/newBlockFilter |

## Request Example

{% tabs %}
{% tab title="cURL" %}
```bash
curl -X POST https://go.getblock.io/<ACCESS-TOKEN>/ \
-H "Content-Type: application/json" \
-d '{
  "jsonrpc": "2.0",
  "id": "getblock.io",
  "method": "eth_getFilterChanges",
  "params": ["0x1"]
}'
```
{% endtab %}

{% tab title="JavaScript (Axios)" %}
```javascript
const axios = require('axios');

const url = 'https://go.getblock.io/<ACCESS-TOKEN>/';

const payload = {
  jsonrpc: '2.0',
  id: 'getblock.io',
  method: 'eth_getFilterChanges',
  params: ['0x1']
};

axios.post(url, payload, {
  headers: { 'Content-Type': 'application/json' }
})
.then(response => console.log(response.data))
.catch(error => console.error(error));
```
{% endtab %}

{% tab title="Python" %}
```python
import requests
import json

url = "https://go.getblock.io/<ACCESS-TOKEN>/"

payload = {
    "jsonrpc": "2.0",
    "id": "getblock.io",
    "method": "eth_getFilterChanges",
    "params": ["0x1"]
}

response = requests.post(url, headers={"Content-Type": "application/json"}, json=payload)
print(response.json())
```
{% endtab %}
{% endtabs %}

## Response Example

{% code title="response.json" overflow="wrap" %}
```json
{
  "jsonrpc": "2.0",
  "id": "getblock.io",
  "result": [
    {
      "address": "0x765DE816845861e75A25fCA122bb6898B8B1282a",
      "topics": ["0xddf252ad1be2c89b69c2b068fc378daa952ba7f163c4a11628f55a4df523b3ef"],
      "data": "0x0000000000000000000000000000000000000000000000000de0b6b3a7640000",
      "blockNumber": "0x1d9f2a8",
      "transactionHash": "0x88df016429689c079f3b2f6ad39fa052532c56795b733da78a91ebe6a713944b"
    }
  ]
}
```
{% endcode %}

## Response Definition

| Field  | Type  | Description                   |
| ------ | ----- | ----------------------------- |
| result | array | Array of logs or block hashes |

## Use Cases

* Poll for new events
* Implement event listeners without WebSocket
* Track stablecoin activity
* Monitor contract events

## Error Handling

| Error Code | Description       |
| ---------- | ----------------- |
| -32602     | Invalid filter ID |
| -32603     | Internal error    |
