---
description: >-
  Example code for the debug_traceCall JSON-RPC method. Complete guide on how to
  use debug_traceCall JSON-RPC in GetBlock Web3 documentation.
---

# debug\_traceCall - Celo

This method traces a call without creating a transaction on the Celo network. This is useful for simulating and debugging contract calls before submitting transactions.

## Parameters

| Parameter   | Type   | Required | Description              |
| ----------- | ------ | -------- | ------------------------ |
| callObject  | object | Yes      | Transaction call object  |
| blockNumber | string | Yes      | Block number or "latest" |
| options     | object | No       | Tracer configuration     |

## Request Example

{% tabs %}
{% tab title="cURL" %}
{% code title="cURL" overflow="wrap" %}
```bash
curl -X POST https://go.getblock.io/<ACCESS-TOKEN>/ \
-H "Content-Type: application/json" \
-d '{
  "jsonrpc": "2.0",
  "id": "getblock.io",
  "method": "debug_traceCall",
  "params": [
    {
      "to": "0x765DE816845861e75A25fCA122bb6898B8B1282a",
      "data": "0x70a08231000000000000000000000000742d35cc6634c0532925a3b844bc9e7595f8bb45"
    },
    "latest",
    {"tracer": "callTracer"}
  ]
}'
```
{% endcode %}
{% endtab %}

{% tab title="JavaScript (Axios)" %}
{% code title="debug_traceCall.js" %}
```javascript
const axios = require('axios');

const url = 'https://go.getblock.io/<ACCESS-TOKEN>/';

const payload = {
  jsonrpc: '2.0',
  id: 'getblock.io',
  method: 'debug_traceCall',
  params: [
    {
      to: '0x765DE816845861e75A25fCA122bb6898B8B1282a',
      data: '0x70a08231000000000000000000000000742d35cc6634c0532925a3b844bc9e7595f8bb45'
    },
    'latest',
    { tracer: 'callTracer' }
  ]
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
{% code title="debug_traceCall.py" %}
```python
import requests
import json

url = "https://go.getblock.io/<ACCESS-TOKEN>/"

payload = {
    "jsonrpc": "2.0",
    "id": "getblock.io",
    "method": "debug_traceCall",
    "params": [
        {
            "to": "0x765DE816845861e75A25fCA122bb6898B8B1282a",
            "data": "0x70a08231000000000000000000000000742d35cc6634c0532925a3b844bc9e7595f8bb45"
        },
        "latest",
        {"tracer": "callTracer"}
    ]
}

response = requests.post(url, headers={"Content-Type": "application/json"}, json=payload)
print(json.dumps(response.json(), indent=2))
```
{% endcode %}
{% endtab %}
{% endtabs %}

## Response Example

{% code title="Response (JSON)" %}
```json
{
  "jsonrpc": "2.0",
  "id": "getblock.io",
  "result": {
    "type": "CALL",
    "from": "0x0000000000000000000000000000000000000000",
    "to": "0x765de816845861e75a25fca122bb6898b8b1282a",
    "gas": "0x2faf080",
    "gasUsed": "0x5a0",
    "input": "0x70a08231...",
    "output": "0x0000000000000000000000000000000000000000000000000de0b6b3a7640000"
  }
}
```
{% endcode %}

## Response Definition

| Field  | Type   | Description     |
| ------ | ------ | --------------- |
| result | object | Execution trace |

## Use Cases

* Simulate contract calls
* Debug before submitting transactions
* Analyze gas usage
* Test cUSD/cEUR interactions

## Error Handling

| Error Code | Description     |
| ---------- | --------------- |
| -32602     | Invalid params  |
| -32603     | Internal error  |
| -32000     | Execution error |
