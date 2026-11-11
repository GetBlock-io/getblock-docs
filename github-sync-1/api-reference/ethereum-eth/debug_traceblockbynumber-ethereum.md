---
description: >-
  Example code for the debug_traceBlockByNumber JSON RPC method. Сomplete guide
  on how to use debug_traceBlockByNumber JSON RPC in GetBlock Web3
  documentation.
---

# debug\_traceBlockByNumber -Ethereum

This method traces the execution of every transaction in a block identified by number or tag. Same output as `debug_traceBlockByHash` but with the convenient block-number-based lookup. The most common entry point when iterating through historical blocks for batch trace analysis.

## Parameters

| Parameter        | Type   | Required | Description                                                                  |
| ---------------- | ------ | -------- | ---------------------------------------------------------------------------- |
| `blockParameter` | string | Yes      | Block number in hex, or `latest`, `earliest`, `pending`, `finalized`, `safe` |
| `tracerOptions`  | object | No       | Trace configuration — same shape as in `debug_traceBlock`                    |

## Request

{% tabs %}
{% tab title="cURL" %}
```bash
curl --location --request POST 'https://shared.eu-central-1.getblock.io/<ACCESS-TOKEN>/' \
--header 'Content-Type: application/json' \
--data-raw '{
    "jsonrpc": "2.0",
    "method": "debug_traceBlockByNumber",
    "params": [
        "0x1720340",
        {
            "tracer": "callTracer"
        }
    ],
    "id": "getblock.io"
}'
```
{% endtab %}

{% tab title="Axios" %}
```javascript
const axios = require('axios');

const response = await axios.post('https://shared.eu-central-1.getblock.io/<ACCESS-TOKEN>/', {
    jsonrpc: '2.0',
    method: 'debug_traceBlockByNumber',
    params: [
        "0x1720340",
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
{% endtab %}

{% tab title="Request" %}
```python
import requests

response = requests.post(
    'https://shared.eu-central-1.getblock.io/<ACCESS-TOKEN>/',
    headers={'Content-Type': 'application/json'},
    json={
        'jsonrpc': '2.0',
        'method': 'debug_traceBlockByNumber',
        'params': [
        "0x1720340",
        {
            "tracer": "callTracer"
        }
    ],
        'id': 'getblock.io'
    }
)

print(response.json())
```
{% endtab %}

{% tab title="Rust" %}
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
            "method": "debug_traceBlockByNumber",
            "params": [
        "0x1720340",
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

* **Historical Backfill**: Iterate through historical block numbers to build a complete internal-call graph database
* **Analytics Pipelines**: Feed trace data into indexers tracking token flows, protocol usage, or MEV events
* **Recent Block Analysis**: Trace the latest block for real-time protocol monitoring

## Error Handling

| Error Code | Message          | Description                                               |
| ---------- | ---------------- | --------------------------------------------------------- |
| -32601     | Method not found | `debug` module not enabled — Dedicated Node tier required |
| -32602     | Invalid params   | Block parameter is malformed                              |
| -32603     | Internal error   | Node failed to execute the trace                          |

## Web3 Integration

{% tabs %}
{% tab title="Ethers.js" %}
```javascript
import { ethers } from 'ethers';

const provider = new ethers.JsonRpcProvider('https://shared.eu-central-1.getblock.io/<ACCESS-TOKEN>/');

const traces = await provider.send('debug_traceBlockByNumber', [
    'latest',
    { tracer: 'callTracer' }
]);
console.log('Traces:', traces.length);
```
{% endtab %}

{% tab title="Viem" %}
```javascript
import { createPublicClient, http } from 'viem';
import { mainnet } from 'viem/chains';

const client = createPublicClient({
    chain: mainnet,
    transport: http('https://shared.eu-central-1.getblock.io/<ACCESS-TOKEN>/'),
});

const traces = await client.request({
    method: 'debug_traceBlockByNumber',
    params: ['latest', { tracer: 'callTracer' }],
});
console.log('Traces:', traces.length);
```
{% endtab %}
{% endtabs %}
