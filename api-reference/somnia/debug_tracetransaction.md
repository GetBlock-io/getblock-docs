---
description: >-
  Example code for the debug_traceTransaction JSON-RPC method. Complete guide on
  how to use debug_traceTransaction JSON-RPC in GetBlock Web3 documentation
---

# debug\_traceTransaction

This method returns a detailed trace of a transaction's execution on the Somnia network. This is essential for debugging failed transactions, analyzing gas consumption, and understanding contract behavior.

## Parameters

| Parameter       | Type   | Required | Description                  |
| --------------- | ------ | -------- | ---------------------------- |
| transactionHash | string | Yes      | 32-byte transaction hash     |
| tracerConfig    | object | No       | Tracer configuration options |

### Tracer Options

| Field        | Type   | Description                                 |
| ------------ | ------ | ------------------------------------------- |
| tracer       | string | Tracer type: "callTracer", "prestateTracer" |
| tracerConfig | object | Tracer-specific options                     |

## Request Example

{% tabs %}
{% tab title="cURL" %}
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
{% endtab %}

{% tab title="JavaScript (Axios)" %}
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
    "method": "debug_traceTransaction",
    "params": [
        "0x88df016429689c079f3b2f6ad39fa052532c56795b733da78a91ebe6a713944b",
        {"tracer": "callTracer"}
    ]
}

response = requests.post(url, headers={"Content-Type": "application/json"}, json=payload)
print(response.json())
```
{% endtab %}

{% tab title="Rust" %}
```rust
use reqwest::Client;
use serde_json::json;

#[tokio::main]
async fn main() -> Result<(), Box<dyn std::error::Error>> {
    let client = Client::new();
    
    let payload = json!({
        "jsonrpc": "2.0",
        "id": "getblock.io",
        "method": "debug_traceTransaction",
        "params": [
            "0x88df016429689c079f3b2f6ad39fa052532c56795b733da78a91ebe6a713944b",
            {"tracer": "callTracer"}
        ]
    });

    let response = client
        .post("https://go.getblock.io/<ACCESS-TOKEN>/")
        .header("Content-Type", "application/json")
        .json(&payload)
        .send()
        .await?;

    let result: serde_json::Value = response.json().await?;
    println!("{:#?}", result);
    
    Ok(())
}
```
{% endtab %}
{% endtabs %}

## Response Example

```json
{
  "jsonrpc": "2.0",
  "id": "getblock.io",
  "result": {
    "type": "CALL",
    "from": "0x742d35cc6634c0532925a3b844bc9e7595f8bb45",
    "to": "0x8626f6940e2eb28930efb4cef49b2d1f2c9c1199",
    "value": "0xde0b6b3a7640000",
    "gas": "0x5208",
    "gasUsed": "0x5208",
    "input": "0x",
    "output": "0x"
  }
}
```

## Response Parameters

| Parameter | Type   | Description                    |
| --------- | ------ | ------------------------------ |
| type      | string | Call type (CALL, CREATE, etc.) |
| from      | string | Sender address                 |
| to        | string | Target address                 |
| value     | string | Value transferred              |
| gas       | string | Gas provided                   |
| gasUsed   | string | Gas consumed                   |
| input     | string | Call input data                |
| output    | string | Call output data               |
| calls     | array  | Nested internal calls          |

## Use Cases

* Debug failed transactions
* Analyze internal calls
* Understand gas consumption
* Security auditing
* MEV analysis

## Error Handling

| Error Code | Description                     |
| ---------- | ------------------------------- |
| -32602     | Invalid params - malformed hash |
| -32603     | Internal error - tracing failed |
| -32000     | Transaction not found           |

## SDK Integration

{% tabs %}
{% tab title="Ethers.js" %}
```javascript
const { ethers } = require('ethers');

const provider = new ethers.JsonRpcProvider('https://go.getblock.io/<ACCESS-TOKEN>/');

const trace = await provider.send('debug_traceTransaction', [
  '0x88df016429689c079f3b2f6ad39fa052532c56795b733da78a91ebe6a713944b',
  { tracer: 'callTracer' }
]);
console.log(trace);
```
{% endtab %}

{% tab title="Viem" %}
```javascript
import { createPublicClient, http } from 'viem';

const client = createPublicClient({
  transport: http('https://go.getblock.io/<ACCESS-TOKEN>/')
});

const trace = await client.request({
  method: 'debug_traceTransaction',
  params: [
    '0x88df016429689c079f3b2f6ad39fa052532c56795b733da78a91ebe6a713944b',
    { tracer: 'callTracer' }
  ]
});
console.log(trace);
```
{% endtab %}
{% endtabs %}
