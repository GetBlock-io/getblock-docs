---
description: >-
  Example code for the debug_traceBlock JSON RPC method. Сomplete guide on how
  to use debug_traceBlock JSON RPC in GetBlock Web3 documentation.
---

# debug\_traceBlock - Ethereum

This method traces the execution of every transaction in a block, given the block's RLP-encoded bytes. Returns per-transaction execution traces including internal calls, state changes, and opcode-level operations depending on the tracer used. The RLP-input variant is useful for tracing hypothetical blocks not present in the local chain. Requires the `debug` module (Dedicated Node tier).

## Parameters

| Parameter       | Type   | Required | Description                                                         |
| --------------- | ------ | -------- | ------------------------------------------------------------------- |
| `blockRlp`      | string | Yes      | Hex-encoded RLP of the block to trace                               |
| `tracerOptions` | object | No       | Trace configuration — `{tracer, timeout, tracerConfig}` (see below) |

### Tracer Options

| Field          | Type   | Required | Description                                                                                             |
| -------------- | ------ | -------- | ------------------------------------------------------------------------------------------------------- |
| `tracer`       | string | No       | Tracer name — `callTracer`, `prestateTracer`, `4byteTracer`, `opcodeTracer`, or a JS/WASM tracer script |
| `timeout`      | string | No       | Trace timeout as a duration string (`10s`, `1m`). Defaults to `5s`                                      |
| `tracerConfig` | object | No       | Tracer-specific configuration passed to the tracer                                                      |

## Request

{% tabs %}
{% tab title="cURL" %}
{% code overflow="wrap" %}
```bash
curl --location --request POST 'https://shared.eu-central-1.getblock.io/<ACCESS-TOKEN>/' \
--header 'Content-Type: application/json' \
--data-raw '{
    "jsonrpc": "2.0",
    "method": "debug_traceBlock",
    "params": [
        "0xf90211a0abcd...",
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
    method: 'debug_traceBlock',
    params: [
        "0xf90211a0abcd...",
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
        'method': 'debug_traceBlock',
        'params': [
        "0xf90211a0abcd...",
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
            "method": "debug_traceBlock",
            "params": [
        "0xf90211a0abcd...",
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
    ]
}
```

## Response Parameters

| Parameter | Type   | Description                                                        |
| --------- | ------ | ------------------------------------------------------------------ |
| `jsonrpc` | string | JSON-RPC protocol version ("2.0")                                  |
| `id`      | string | Request identifier matching the request                            |
| `result`  | array  | Array of per-transaction traces — shape depends on the tracer used |

## Use Cases

* **Historical Block Replay**: Trace hypothetical blocks (e.g. from another fork) for what-if analysis
* **Consensus-Client Testing**: Feed hand-crafted RLP blocks into a tracer to test EVM implementations
* **Analytics Backfill**: Trace blocks captured from a peer's gossip to reconstruct execution history

## Error Handling

| Error Code | Message          | Description                                               |
| ---------- | ---------------- | --------------------------------------------------------- |
| -32601     | Method not found | `debug` module not enabled — Dedicated Node tier required |
| -32602     | Invalid params   | Block RLP is malformed or tracer options are invalid      |
| -32603     | Internal error   | Node failed to execute the trace                          |

## Web3 Integration

{% tabs %}
{% tab title="Ethers.js" %}
{% code title="ethers-example.js" %}
```javascript
import { ethers } from 'ethers';

const provider = new ethers.JsonRpcProvider('https://shared.eu-central-1.getblock.io/<ACCESS-TOKEN>/');

const traces = await provider.send('debug_traceBlock', [
    '0xf90211a0abcd...',  // RLP-encoded block
    { tracer: 'callTracer' }
]);
console.log('Traces:', traces.length);
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
    method: 'debug_traceBlock',
    params: ['0xf90211a0abcd...', { tracer: 'callTracer' }],
});
console.log('Traces:', traces.length);
```
{% endcode %}
{% endtab %}
{% endtabs %}
