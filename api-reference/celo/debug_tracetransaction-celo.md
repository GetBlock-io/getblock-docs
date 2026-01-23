---
description: >-
  Example code for the debug_traceTransaction JSON-RPC method. Complete guide on
  how to use debug_traceTransaction JSON-RPC in GetBlock Web3 documentation.
---

# debug\_traceTransaction - Celo

This method returns a detailed trace of a transaction's execution on the Celo network. This is essential for debugging failed transactions, analyzing gas usage, and understanding contract interactions.

## Parameters

| Parameter       | Type   | Required | Description                  |
| --------------- | ------ | -------- | ---------------------------- |
| transactionHash | string | Yes      | 32-byte transaction hash     |
| options         | object | No       | Tracer configuration options |

### Options Object

| Field        | Type   | Description                                 |
| ------------ | ------ | ------------------------------------------- |
| tracer       | string | Tracer type: "callTracer", "prestateTracer" |
| tracerConfig | object | Tracer-specific configuration               |
| timeout      | string | Maximum trace duration                      |

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
  "method": "debug_traceTransaction",
  "params": [
    "0x88df016429689c079f3b2f6ad39fa052532c56795b733da78a91ebe6a713944b",
    {"tracer": "callTracer"}
  ]
}'
```
{% endcode %}
{% endtab %}

{% tab title="Axios" %}
{% code title="request.js" %}
```javascript
const axios = require('axios');

const url = 'https://go.getblock.io/<ACCESS-TOKEN>/';

const payload = {
  jsonrpc: '2.0',
  id: 'getblock.io',
  method: 'debug_traceTransaction',
  params: [
    '0x88df016429689c079f3b2f6ad39fa052532c56795b733da78a91ebe6a713944b',
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

{% tab title="Request" %}
{% code title="request.py" overflow="wrap" %}
```python
import requests
import json

url = "https://go.getblock.io/<ACCESS-TOKEN>/"

payload = {
    "jsonrpc": "2.0",
    "id": "getblock.io",
    "method": "debug_traceTransaction",
    "params": [
        "0x88df016429689c079f3b2f6ad39fa052532c56795b733da78a91ebe6a713944b",
        {"tracer": "callTracer"}
    ]
}

response = requests.post(url, headers={"Content-Type": "application/json"}, json=payload)
print(json.dumps(response.json(), indent=2))
```
{% endcode %}
{% endtab %}
{% endtabs %}

## Response Example (callTracer)

{% code title="response.json" %}
```json
{
  "jsonrpc": "2.0",
  "id": "getblock.io",
  "result": {
    "type": "CALL",
    "from": "0x742d35cc6634c0532925a3b844bc9e7595f8bb45",
    "to": "0x765de816845861e75a25fca122bb6898b8b1282a",
    "value": "0x0",
    "gas": "0x5208",
    "gasUsed": "0x5208",
    "input": "0xa9059cbb...",
    "output": "0x0000000000000000000000000000000000000000000000000000000000000001",
    "calls": []
  }
}
```
{% endcode %}

## Response Definition

| Field  | Type   | Description              |
| ------ | ------ | ------------------------ |
| result | object | Detailed execution trace |

## Use Cases

* Debug failed transactions
* Analyze internal calls
* Track gas consumption
* Understand contract interactions
* Debug cUSD/cEUR transfers

## Error Handling

| Error Code | Description                     |
| ---------- | ------------------------------- |
| -32602     | Invalid params - malformed hash |
| -32603     | Internal error                  |
| -32000     | Transaction not found           |

## SDK Integration

{% tabs %}
{% tab title="Ethers.js" %}
{% code title="ethers.js" %}
```javascript
const { ethers } = require('ethers');

const provider = new ethers.JsonRpcProvider('https://go.getblock.io/<ACCESS-TOKEN>/');

const trace = await provider.send('debug_traceTransaction', [
  '0x88df016429689c079f3b2f6ad39fa052532c56795b733da78a91ebe6a713944b',
  { tracer: 'callTracer' }
]);
console.log(JSON.stringify(trace, null, 2));
```
{% endcode %}
{% endtab %}
{% endtabs %}
