---
description: >-
  Example code for the debug_traceCall JSON RPC method. Сomplete guide on how to
  use debug_traceCall JSON RPC in GetBlock Web3 documentation.
---

# debug\_traceCall - Ethereum

This method simulates a message call at a specific block and returns its execution trace — without submitting a transaction. Unlike `eth_call` (which returns only the final return data), `debug_traceCall` returns the complete internal-call graph, opcode-level operations, or pre-state depending on the tracer. Essential for wallet UX previews, MEV analysis, and pre-flight simulation of complex contract interactions.

## Parameters

| Parameter        | Type   | Required | Description                                                           |
| ---------------- | ------ | -------- | --------------------------------------------------------------------- |
| `transaction`    | object | Yes      | Transaction call object — same shape as `eth_call` transaction object |
| `blockParameter` | string | Yes      | Block number in hex or tag                                            |
| `tracerOptions`  | object | No       | Trace configuration — same shape as in `debug_traceBlock`             |

## Request

{% tabs %}
{% tab title="cURL" %}
{% code overflow="wrap" %}
```bash
curl --location --request POST 'https://shared.eu-central-1.getblock.io/<ACCESS-TOKEN>/' \
--header 'Content-Type: application/json' \
--data-raw '{
    "jsonrpc": "2.0",
    "method": "debug_traceCall",
    "params": [
        {
            "from": "0xd8dA6BF26964aF9D7eEd9e03E53415D37aA96045",
            "to": "0xC02aaA39b223FE8D0A0e5C4F27eAD9083C756Cc2",
            "data": "0x70a08231000000000000000000000000d8da6bf26964af9d7eed9e03e53415d37aa96045"
        },
        "latest",
        {
            "tracer": "callTracer"
        }
    ],
    "id": "getblock.io"
}'
```
{% endcode %}
{% endtab %}

{% tab title="Axios" %}
{% code title="example.js" %}
```javascript
const axios = require('axios');

const response = await axios.post('https://shared.eu-central-1.getblock.io/<ACCESS-TOKEN>/', {
    jsonrpc: '2.0',
    method: 'debug_traceCall',
    params: [
        {
            "from": "0xd8dA6BF26964aF9D7eEd9e03E53415D37aA96045",
            "to": "0xC02aaA39b223FE8D0A0e5C4F27eAD9083C756Cc2",
            "data": "0x70a08231000000000000000000000000d8da6bf26964af9d7eed9e03e53415d37aa96045"
        },
        "latest",
        {
            "tracer": "callTracer"
        }
    ],
    id: 'getblock.io'
}, {
    headers: { 'Content-Type': 'application/json' }
});

console.log(response.data.result);
```
{% endcode %}
{% endtab %}

{% tab title="Request" %}
{% code title="example.py" %}
```python
import requests

response = requests.post(
    'https://shared.eu-central-1.getblock.io/<ACCESS-TOKEN>/',
    headers={'Content-Type': 'application/json'},
    json={
        'jsonrpc': '2.0',
        'method': 'debug_traceCall',
        'params': [
            {
                "from": "0xd8dA6BF26964aF9D7eEd9e03E53415D37aA96045",
                "to": "0xC02aaA39b223FE8D0A0e5C4F27eAD9083C756Cc2",
                "data": "0x70a08231000000000000000000000000d8da6bf26964af9d7eed9e03e53415d37aa96045"
            },
            "latest",
            {
                "tracer": "callTracer"
            }
        ],
        'id': 'getblock.io'
    }
)

print(response.json())
```
{% endcode %}
{% endtab %}

{% tab title="Rust" %}
{% code title="example.rs" %}
```rust
use reqwest::Client;
use serde_json::{json, Value};

#[tokio::main]
async fn main() -> Result<(), Box<dyn std::error::Error>> {
    let client = Client::new();
    
    let response = client
        .post("https://shared.eu-central-1.getblock.io/<ACCESS-TOKEN>/")
        .header("Content-Type", "application/json")
        .json(&json!({
            "jsonrpc": "2.0",
            "method": "debug_traceCall",
            "params": [
                {
                    "from": "0xd8dA6BF26964aF9D7eEd9e03E53415D37aA96045",
                    "to": "0xC02aaA39b223FE8D0A0e5C4F27eAD9083C756Cc2",
                    "data": "0x70a08231000000000000000000000000d8da6bf26964af9d7eed9e03e53415d37aa96045"
                },
                "latest",
                {
                    "tracer": "callTracer"
                }
            ],
            "id": "getblock.io"
        }))
        .send()
        .await?
        .json::<Value>()
        .await?;
    
    println!("Result: {}", response["result"]);
    Ok(())
}
```
{% endcode %}
{% endtab %}
{% endtabs %}

## Response

```json
{
    "jsonrpc": "2.0",
    "id": "getblock.io",
    "result": {
        "type": "CALL",
        "from": "0xd8dA6BF26964aF9D7eEd9e03E53415D37aA96045",
        "to": "0xC02aaA39b223FE8D0A0e5C4F27eAD9083C756Cc2",
        "value": "0x0",
        "gas": "0x186a0",
        "gasUsed": "0x8ba0",
        "input": "0x70a08231000000000000000000000000d8da6bf26964af9d7eed9e03e53415d37aa96045",
        "output": "0x0000000000000000000000000000000000000000000000000de0b6b3a7640000",
        "calls": []
    }
}
```

## Response Parameters

| Parameter | Type            | Description                                                                               |
| --------- | --------------- | ----------------------------------------------------------------------------------------- |
| `jsonrpc` | string          | JSON-RPC protocol version ("2.0")                                                         |
| `id`      | string          | Request identifier matching the request                                                   |
| `result`  | object or array | Trace result — shape depends on the tracer used (`callTracer` returns a call tree object) |

## Use Cases

* **Wallet UX Preview**: Show users a complete internal-call graph before they sign — every contract interaction, every value transfer
* **MEV Simulation**: Trace hypothetical transactions against current state to identify sandwich or backrun opportunities
* **Debugging Reverts**: Trace a reverting call to identify the exact contract, function, and opcode where revert occurred
* **Gas Attribution**: Break down gas cost across internal calls to identify optimization targets

## Error Handling

| Error Code | Message            | Description                                                                       |
| ---------- | ------------------ | --------------------------------------------------------------------------------- |
| `-32601`   | Method not found   | `debug` module not enabled — Dedicated Node tier required                         |
| `-32602`   | Invalid params     | Transaction object or tracer options are malformed                                |
| `3`        | Execution reverted | The traced call reverted — the trace still returns useful revert-path information |
| `-32603`   | Internal error     | Node failed to execute the trace                                                  |

## Web3 Integration

{% tabs %}
{% tab title="Ethers.js" %}
{% code title="ethers-example.js" %}
```javascript
import { ethers } from 'ethers';

const provider = new ethers.JsonRpcProvider('https://shared.eu-central-1.getblock.io/<ACCESS-TOKEN>/');

const trace = await provider.send('debug_traceCall', [
    { from: '0xd8dA6BF26964aF9D7eEd9e03E53415D37aA96045', to: '0xC02aaA39b223FE8D0A0e5C4F27eAD9083C756Cc2', data: '0x70a08231000000000000000000000000d8da6bf26964af9d7eed9e03e53415d37aa96045' },
    'latest',
    { tracer: 'callTracer' }
]);
console.log('Top-level call type:', trace.type);
console.log('Gas used:', trace.gasUsed);
console.log('Internal calls:', trace.calls?.length ?? 0);
```
{% endcode %}
{% endtab %}

{% tab title="Viem" %}
{% code title="viem-example.js" %}
```javascript
import { createPublicClient, http } from 'viem';
import { mainnet } from 'viem/chains';

const client = createPublicClient({
    chain: mainnet,
    transport: http('https://shared.eu-central-1.getblock.io/<ACCESS-TOKEN>/'),
});

const trace = await client.request({
    method: 'debug_traceCall',
    params: [
        { from: '0xd8dA6BF26964aF9D7eEd9e03E53415D37aA96045', to: '0xC02aaA39b223FE8D0A0e5C4F27eAD9083C756Cc2', data: '0x70a08231000000000000000000000000d8da6bf26964af9d7eed9e03e53415d37aa96045' },
        'latest',
        { tracer: 'callTracer' },
    ],
});
console.log('Top-level call type:', trace.type);
```
{% endcode %}
{% endtab %}
{% endtabs %}
