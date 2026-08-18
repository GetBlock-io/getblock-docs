---
description: >-
  Example code for the debug_traceBlockByHash JSON-RPC method. Сomplete guide on
  how to use debug_traceBlockByHash JSON-RPC in GetBlock.io Web3 documentation.
---

# debug\_traceBlockByHash - Unichain

This method replays every transaction in a block identified by hash and returns an execution trace for each. It is used to analyze all execution in a block at once.

## Parameters

| Parameter | Type   | Required | Description                                                   |
| --------- | ------ | -------- | ------------------------------------------------------------- |
| blockHash | string | Yes      | 32-byte block hash                                            |
| options   | object | No       | Tracer options, such as the tracer name and its configuration |

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
    "params": ["0x8e3f5b2a9c1d4e7f0a6b3c8d5e2f9a1b4c7d0e3f6a9b2c5d8e1f4a7b0c3d6e9f", {"tracer": "callTracer"}],
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
    params: ["0x8e3f5b2a9c1d4e7f0a6b3c8d5e2f9a1b4c7d0e3f6a9b2c5d8e1f4a7b0c3d6e9f", {"tracer": "callTracer"}],
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
        'params': ["0x8e3f5b2a9c1d4e7f0a6b3c8d5e2f9a1b4c7d0e3f6a9b2c5d8e1f4a7b0c3d6e9f", {"tracer": "callTracer"}],
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
            "params": ["0x8e3f5b2a9c1d4e7f0a6b3c8d5e2f9a1b4c7d0e3f6a9b2c5d8e1f4a7b0c3d6e9f", {"tracer": "callTracer"}],
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
            "txHash": "0x4a7b0c3d6e9f2a5b8c1d4e7f0a3b6c9d2e5f8a1b4c7d0e3f6a9b2c5d8e1f4a7b",
            "result": {
                "from": "0xd8dA6BF26964aF9D7eEd9e03E53415D37aA96045",
                "to": "0x4200000000000000000000000000000000000006",
                "gasUsed": "0x5208",
                "output": "0x",
                "type": "CALL",
                "calls": []
            }
        }
    ]
}
```

## Response Parameters

| Parameter | Type   | Description                                   |
| --------- | ------ | --------------------------------------------- |
| jsonrpc   | string | JSON-RPC protocol version ("2.0")             |
| id        | string | Request identifier matching the request       |
| result    | array  | Array of per-transaction traces for the block |

### Trace Entry

| Field  | Type   | Description                                               |
| ------ | ------ | --------------------------------------------------------- |
| txHash | string | Hash of the traced transaction                            |
| result | object | Execution trace for the transaction, shaped by the tracer |

## Use Cases

* **Block Analysis**: Trace all execution in a block by hash
* **Reorg-Safe Tracing**: Pin a trace to a specific block hash
* **Internal Transfers**: Extract internal value transfers across a block
* **Auditing**: Review every call frame produced in a block

## Error Handling

| Error Code | Message        | Description                                           |
| ---------- | -------------- | ----------------------------------------------------- |
| -32602     | Invalid params | The block number or hash is malformed or out of range |
| -32603     | Internal error | The node failed to read the block                     |

## Web3 Integration

{% tabs %}
{% tab title="Ethers.js" %}
{% code title="ethers-example.js" %}
```javascript
const { ethers } = require('ethers');

const provider = new ethers.JsonRpcProvider('https://shared.eu-central-1.getblock.io/<ACCESS-TOKEN>/');

const traces = await provider.send('debug_traceBlockByHash', ['0x8e3f5b2a9c1d4e7f0a6b3c8d5e2f9a1b4c7d0e3f6a9b2c5d8e1f4a7b0c3d6e9f', { tracer: 'callTracer' }]);
console.log(traces);
```
{% endcode %}
{% endtab %}

{% tab title="Viem" %}
{% code title="viem-example.js" %}
```javascript
import { createPublicClient, http } from 'viem';
import { unichain } from 'viem/chains';

const client = createPublicClient({ chain: unichain, transport: http('https://shared.eu-central-1.getblock.io/<ACCESS-TOKEN>/') });

const traces = await client.request({ method: 'debug_traceBlockByHash', params: ['0x8e3f5b2a9c1d4e7f0a6b3c8d5e2f9a1b4c7d0e3f6a9b2c5d8e1f4a7b0c3d6e9f', { tracer: 'callTracer' }] });
console.log(traces);
```
{% endcode %}
{% endtab %}
{% endtabs %}
