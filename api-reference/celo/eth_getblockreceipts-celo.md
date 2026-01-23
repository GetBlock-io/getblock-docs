---
description: >-
  Example code for the eth_getBlockReceipts JSON-RPC method. Complete guide on
  how to use eth_getBlockReceipts JSON-RPC in GetBlock Web3 documentation.
---

# eth\_getBlockReceipts - Celo

This method returns all transaction receipts for a given block on the Celo network. This is useful for batch processing of transaction data and building block explorers.

## Parameters

| Parameter   | Type   | Required | Description                                             |
| ----------- | ------ | -------- | ------------------------------------------------------- |
| blockNumber | string | Yes      | Block number in hex, or "latest", "earliest", "pending" |

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
  "method": "eth_getBlockReceipts",
  "params": ["latest"]
}'
```
{% endcode %}
{% endtab %}

{% tab title="JavaScript (Axios)" %}
{% code title="example.js" %}
```javascript
const axios = require('axios');

const url = 'https://go.getblock.io/<ACCESS-TOKEN>/';

const payload = {
  jsonrpc: '2.0',
  id: 'getblock.io',
  method: 'eth_getBlockReceipts',
  params: ['latest']
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
    "method": "eth_getBlockReceipts",
    "params": ["latest"]
}

response = requests.post(url, headers={"Content-Type": "application/json"}, json=payload)
print(response.json())
```
{% endcode %}
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
      "transactionHash": "0x88df016429689c079f3b2f6ad39fa052532c56795b733da78a91ebe6a713944b",
      "blockNumber": "0x1d9f2a8",
      "gasUsed": "0x5208",
      "status": "0x1",
      "logs": []
    }
  ]
}
```
{% endcode %}

## Response Definition

| Field  | Type  | Description                          |
| ------ | ----- | ------------------------------------ |
| result | array | Array of transaction receipt objects |

## Use Cases

* Batch process transaction receipts
* Build block explorers
* Index all block transactions
* Calculate gas statistics

## Error Handling

| Error Code  | Description     |
| ----------- | --------------- |
| -32602      | Invalid params  |
| -32603      | Internal error  |
| null result | Block not found |
