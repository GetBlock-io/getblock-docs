---
description: >-
  Example code for the debug_traceBlockByHash JSON RPC method. Сomplete guide on
  how to use debug_traceBlockByHash JSON RPC in GetBlock Web3 documentation.
---

# debug\_traceBlockByHash - Ethereum

This method traces the execution of every transaction in a block identified by its hash. Returns per-transaction execution traces including internal calls (`callTracer`), pre-state (`prestateTracer`), or opcode-level operations. The most common tracing entry point for retrospective analysis of confirmed blocks.

## Parameters

| Parameter       | Type   | Required | Description                                               |
| --------------- | ------ | -------- | --------------------------------------------------------- |
| `blockHash`     | string | Yes      | 32-byte block hash                                        |
| `tracerOptions` | object | No       | Trace configuration — same shape as in `debug_traceBlock` |

### Tracer Options

| Field          | Type   | Required | Description                                                                 |
| -------------- | ------ | -------- | --------------------------------------------------------------------------- |
| `tracer`       | string | No       | Tracer name — `callTracer`, `prestateTracer`, `4byteTracer`, `opcodeTracer` |
| `timeout`      | string | No       | Trace timeout as a duration string. Defaults to `5s`                        |
| `tracerConfig` | object | No       | Tracer-specific configuration                                               |

## Request

{% tabs %}
{% tab title="cURL" %}
{% code overflow="wrap" %}
```bash
curl --location --request POST 'https://shared.eu-central-1.getblock.io/<ACCESS-TOKEN>/' \
--header 'Content-Type: application/json' \
--data-raw '{
    "jsonrpc": "2.0",
    "method": "debug_traceBlockByHash",
    "params": [
        "0x9ec8e2f6b78d8f5f2ac3e5d61b0d38d4d5a8f0e7c8b5a2f9d3e6c1b7a4d0f2e5c",
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
    method: 'debug_traceBlockByHash',
    params: [
        "0x9ec8e2f6b78d8f5f2ac3e5d61b0d38d4d5a8f0e7c8b5a2f9d3e6c1b7a4d0f2e5c",
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
        'method': 'debug_traceBlockByHash',
        'params': [
        "0x9ec8e2f6b78d8f5f2ac3e5d61b0d38d4d5a8f0e7c8b5a2f9d3e6c1b7a4d0f2e5c",
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
            "method": "debug_traceBlockByHash",
            "params": [
        "0x9ec8e2f6b78d8f5f2ac3e5d61b0d38d4d5a8f0e7c8b5a2f9d3e6c1b7a4d0f2e5c",
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
    "result": [
        {
            "txHash": "0x88df016429689c079f3b2f6ad39fa052532c56795b733da78a91ebe6a713944b",
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
    ]
}
```

## Response Parameters

| Parameter | Type            | Description                                                            |
| --------- | --------------- | ---------------------------------------------------------------------- |
| `jsonrpc` | string          | JSON-RPC protocol version ("2.0")                                      |
| `id`      | string          | Request identifier matching the request                                |
| `result`  | array of object | Array of `{txHash, result}` records — one per transaction in the block |

## Use Cases

* **Full-Block Analysis**: Trace all transactions in a specific historical block by its hash
* **MEV Post-Mortem**: Analyze a block's complete internal-call graph for MEV extraction analysis
* **Contract Interaction Auditing**: Enumerate every internal call to a target contract within a block
* **Exploit Investigation**: Trace a block containing an exploit transaction to understand the full attack path

## Error Handling

| Error Code | Message          | Description                                                                             |
| ---------- | ---------------- | --------------------------------------------------------------------------------------- |
| -32601     | Method not found | `debug` module not enabled — Dedicated Node tier required                               |
| -32602     | Invalid params   | Block hash is malformed or tracer options are invalid                                   |
| -32603     | Internal error   | Node failed to execute the trace (often indicates state pruning at the requested block) |

## Web3 Integration

{% tabs %}
{% tab title="Ethers.js" %}
{% code title="ethers-example.js" %}
```javascript
import { ethers } from 'ethers';

const provider = new ethers.JsonRpcProvider('https://shared.eu-central-1.getblock.io/<ACCESS-TOKEN>/');

const traces = await provider.send('debug_traceBlockByHash', [
    '0x9ec8e2f6b78d8f5f2ac3e5d61b0d38d4d5a8f0e7c8b5a2f9d3e6c1b7a4d0f2e5c',
    { tracer: 'callTracer' }
]);
console.log('Traces:', traces.length);
traces.forEach(t => console.log(t.txHash, t.result.type));
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

const traces = await client.request({
    method: 'debug_traceBlockByHash',
    params: ['0x9ec8e2f6b78d8f5f2ac3e5d61b0d38d4d5a8f0e7c8b5a2f9d3e6c1b7a4d0f2e5c', { tracer: 'callTracer' }],
});
console.log('Traces:', traces.length);
```
{% endcode %}
{% endtab %}
{% endtabs %}
