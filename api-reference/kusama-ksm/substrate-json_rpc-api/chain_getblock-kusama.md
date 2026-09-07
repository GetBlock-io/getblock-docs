---
description: >-
  Example code for the chain_getBlock JSON-RPC method. Complete guide on how to
  use chain_getBlock JSON-RPC in GetBlock Web3 documentation.
---

# chain\_getBlock - Kusama

Returns the full block (header and extrinsics) for a given block hash, or the latest block when the hash is omitted.

{% hint style="info" %}
Available over both the JSON-RPC (HTTP) and WebSocket interfaces.
{% endhint %}

## Parameters

| Parameter | Type   | Required | Description                           |
| --------- | ------ | -------- | ------------------------------------- |
| hash      | string | Optional | Block hash; omit for the latest block |

## Request

{% tabs %}
{% tab title="cURL" %}
{% code overflow="wrap" %}
```bash
curl --location --request POST 'https://shared.eu-central-1.getblock.io/<ACCESS-TOKEN>/' \
--header 'Content-Type: application/json' \
--data-raw '{
    "jsonrpc": "2.0",
    "id": "getblock.io",
    "method": "chain_getBlock",
    "params": [
    "0x8a1e6d2f0b7c4a9e3d5f1b8c2a6e4d0f9b3c7a5e1d8f2b6c4a0e9d3f7b1c5a2e6"
]
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
    id: 'getblock.io',
    method: 'chain_getBlock',
    params: [
    "0x8a1e6d2f0b7c4a9e3d5f1b8c2a6e4d0f9b3c7a5e1d8f2b6c4a0e9d3f7b1c5a2e6"
]
}, { headers: { 'Content-Type': 'application/json' } });

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
        'id': 'getblock.io',
        'method': 'chain_getBlock',
        'params': [
    "0x8a1e6d2f0b7c4a9e3d5f1b8c2a6e4d0f9b3c7a5e1d8f2b6c4a0e9d3f7b1c5a2e6"
]
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
        .json(&json!({
            "jsonrpc": "2.0",
            "id": "getblock.io",
            "method": "chain_getBlock",
            "params": [
    "0x8a1e6d2f0b7c4a9e3d5f1b8c2a6e4d0f9b3c7a5e1d8f2b6c4a0e9d3f7b1c5a2e6"
]
        }))
        .send().await?
        .json::<Value>().await?;
    println!("{}", response["result"]);
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
        "block": {
            "header": {
                "parentHash": "0x...",
                "number": "0x17f7b40",
                "stateRoot": "0x...",
                "extrinsicsRoot": "0x...",
                "digest": {
                    "logs": [
                        "0x..."
                    ]
                }
            },
            "extrinsics": [
                "0x280403..."
            ]
        },
        "justifications": null
    }
}
```

## Response Fields

| Field            | Type   | Description                                      |
| ---------------- | ------ | ------------------------------------------------ |
| block.header     | object | Block header (parentHash, number, roots, digest) |
| block.extrinsics | array  | SCALE-encoded extrinsics (hex) in the block      |
| justifications   | array  | null                                             |

## Use Cases

* **Block Inspection**: Read a block's extrinsics and header
* **Indexing**: Decode extrinsics for an indexer
* **Explorers**: Render block detail pages

## Error Handling

| Error                     | Message       | Description                                       |
| ------------------------- | ------------- | ------------------------------------------------- |
| -32603 / Internal error   | Unknown block | No block matches the supplied hash                |
| 403 / RBAC: access denied | Access denied | The GetBlock access token is missing or incorrect |
