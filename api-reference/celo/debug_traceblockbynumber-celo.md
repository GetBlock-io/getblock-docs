---
description: >-
  Example code for the debug_traceBlockByNumber JSON-RPC method. Complete guide
  on how to use debug_traceBlockByNumber JSON-RPC in GetBlock Web3
  documentation.
---

# debug\_traceBlockByNumber - Celo

This method traces all transactions in a block by its number on the Celo network. It returns detailed execution information for every transaction in the block.

## Parameters

| Parameter   | Type   | Required | Description                     |
| ----------- | ------ | -------- | ------------------------------- |
| blockNumber | string | Yes      | Block number in hex or "latest" |
| options     | object | No       | Tracer configuration options    |

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
  "method": "debug_traceBlockByNumber",
  "params": ["0x1d9f2a8", {"tracer": "callTracer"}]
}'
```
{% endcode %}
{% endtab %}

{% tab title="JavaScript (Axios)" %}
{% code title="traceBlock.js" %}
```javascript
const axios = require('axios');

const url = 'https://go.getblock.io/<ACCESS-TOKEN>/';

const payload = {
  jsonrpc: '2.0',
  id: 'getblock.io',
  method: 'debug_traceBlockByNumber',
  params: ['0x1d9f2a8', { tracer: 'callTracer' }]
};

axios.post(url, payload, {
  headers: { 'Content-Type': 'application/json' }
})
.then(response => console.log(JSON.stringify(response.data, null, 2)))
.catch(error => console.error(error));
```
{% endcode %}
{% endtab %}

{% tab title="Python" %}
{% code title="trace_block.py" %}
```python
import requests
import json

url = "https://go.getblock.io/<ACCESS-TOKEN>/"

payload = {
    "jsonrpc": "2.0",
    "id": "getblock.io",
    "method": "debug_traceBlockByNumber",
    "params": ["0x1d9f2a8", {"tracer": "callTracer"}]
}

response = requests.post(url, headers={"Content-Type": "application/json"}, json=payload)
print(json.dumps(response.json(), indent=2))
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
  "result": [
    {
      "txHash": "0x88df016429689c079f3b2f6ad39fa052532c56795b733da78a91ebe6a713944b",
      "result": {
        "type": "CALL",
        "from": "0x742d35cc6634c0532925a3b844bc9e7595f8bb45",
        "to": "0x765de816845861e75a25fca122bb6898b8b1282a",
        "gasUsed": "0x5208"
      }
    }
  ]
}
```
{% endcode %}

## Returns

| Field  | Type  | Description                 |
| ------ | ----- | --------------------------- |
| result | array | Array of transaction traces |

## Use Cases

* Analyze entire block execution
* Debug block-level issues
* Build analytics tools
* Index transaction traces

## Error Handling

| Error Code  | Description     |
| ----------- | --------------- |
| -32602      | Invalid params  |
| -32603      | Internal error  |
| null result | Block not found |
