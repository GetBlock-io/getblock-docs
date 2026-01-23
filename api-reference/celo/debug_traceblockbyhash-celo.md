---
description: >-
  Example code for the debug_traceBlockByHash JSON-RPC method. Complete guide on
  how to use debug_traceBlockByHash JSON-RPC in GetBlock Web3 documentation.
---

# debug\_traceBlockByHash - Celo

This method traces all transactions in a block by its hash on the Celo network. Similar to `debug_traceBlockByNumber` but uses block hash for identification.

## Parameters

| Parameter | Type   | Required | Description                  |
| --------- | ------ | -------- | ---------------------------- |
| blockHash | string | Yes      | 32-byte block hash           |
| options   | object | No       | Tracer configuration options |

## Request Example

{% tabs %}
{% tab title="cURL" %}
```bash
curl -X POST https://go.getblock.io/<ACCESS-TOKEN>/ \
-H "Content-Type: application/json" \
-d '{
  "jsonrpc": "2.0",
  "id": "getblock.io",
  "method": "debug_traceBlockByHash",
  "params": [
    "0x4e3a3754410177e6937ef1f84bba68ea139e8d1a2258c5f85db9f1cd715a1bdd",
    {"tracer": "callTracer"}
  ]
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
  method: 'debug_traceBlockByHash',
  params: [
    '0x4e3a3754410177e6937ef1f84bba68ea139e8d1a2258c5f85db9f1cd715a1bdd',
    { tracer: 'callTracer' }
  ]
};

axios.post(url, payload, {
  headers: { 'Content-Type': 'application/json' }
})
.then(response => console.log(JSON.stringify(response.data, null, 2)))
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
    "method": "debug_traceBlockByHash",
    "params": [
        "0x4e3a3754410177e6937ef1f84bba68ea139e8d1a2258c5f85db9f1cd715a1bdd",
        {"tracer": "callTracer"}
    ]
}

response = requests.post(url, headers={"Content-Type": "application/json"}, json=payload)
print(json.dumps(response.json(), indent=2))
```
{% endtab %}
{% endtabs %}

## Response Example

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

## Response Definition

| Field  | Type  | Description                 |
| ------ | ----- | --------------------------- |
| result | array | Array of transaction traces |

## Use Cases

* Debug specific block by hash
* Verify block execution
* Analyze historical blocks
* Build block tracing tools

## Error Handling

| Error Code  | Description     |
| ----------- | --------------- |
| -32602      | Invalid params  |
| -32603      | Internal error  |
| null result | Block not found |
