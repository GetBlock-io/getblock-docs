---
description: >-
  Example code for the eth_getBlockByHash JSON-RPC method. Complete guide on how
  to use eth_getBlockByHash JSON-RPC in GetBlock Web3 documentation.
---

# eth\_getBlockByHash - Sonic

This method returns information about a block by its hash. The second parameter controls whether full transaction objects or only transaction hashes are returned.

## Parameters

| Parameter        | Type    | Required | Description                                              |
| ---------------- | ------- | -------- | -------------------------------------------------------- |
| blockHash        | string  | Yes      | 32-byte block hash                                       |
| fullTransactions | boolean | Yes      | true for full transaction objects, false for hashes only |

## Request

{% tabs %}
{% tab title="cURL" %}
{% code overflow="wrap" %}
```bash
curl --location --request POST 'https://go.getblock.io/<ACCESS-TOKEN>/' \
--header 'Content-Type: application/json' \
--data-raw '{
    "jsonrpc": "2.0",
    "method": "eth_getBlockByHash",
    "params": ["0x8e3f5b2a9c1d4e7f0a6b3c8d5e2f9a1b4c7d0e3f6a9b2c5d8e1f4a7b0c3d6e9f", false],
    "id": "getblock.io"
}'
```
{% endcode %}
{% endtab %}

{% tab title="Axios" %}
{% code title="example.js" %}
```javascript
const axios = require('axios');

const response = await axios.post('https://go.getblock.io/<ACCESS-TOKEN>/', {
    jsonrpc: '2.0',
    method: 'eth_getBlockByHash',
    params: ["0x8e3f5b2a9c1d4e7f0a6b3c8d5e2f9a1b4c7d0e3f6a9b2c5d8e1f4a7b0c3d6e9f", false],
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
    'https://go.getblock.io/<ACCESS-TOKEN>/',
    headers={'Content-Type': 'application/json'},
    json={
        'jsonrpc': '2.0',
        'method': 'eth_getBlockByHash',
        'params': ["0x8e3f5b2a9c1d4e7f0a6b3c8d5e2f9a1b4c7d0e3f6a9b2c5d8e1f4a7b0c3d6e9f", false],
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
        .post("https://go.getblock.io/<ACCESS-TOKEN>/")
        .header("Content-Type", "application/json")
        .json(&json!({
            "jsonrpc": "2.0",
            "method": "eth_getBlockByHash",
            "params": ["0x8e3f5b2a9c1d4e7f0a6b3c8d5e2f9a1b4c7d0e3f6a9b2c5d8e1f4a7b0c3d6e9f", false],
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
        "number": "0x1e8480",
        "hash": "0x8e3f5b2a9c1d4e7f0a6b3c8d5e2f9a1b4c7d0e3f6a9b2c5d8e1f4a7b0c3d6e9f",
        "parentHash": "0x7d2c690aed07954c3862f6600a7e9e63d0474412a9c1d4e7f0a6b3c8d5e2f9a1b",
        "timestamp": "0x66f5a2c0",
        "gasLimit": "0x1c9c380",
        "gasUsed": "0xb3f7a",
        "baseFeePerGas": "0x3b9aca00",
        "size": "0x2a1f",
        "transactions": [
            "0x4a7b0c3d6e9f2a5b8c1d4e7f0a3b6c9d2e5f8a1b4c7d0e3f6a9b2c5d8e1f4a7b"
        ]
    }
}
```

## Response Parameters

| Parameter | Type   | Description                                       |
| --------- | ------ | ------------------------------------------------- |
| jsonrpc   | string | JSON-RPC protocol version ("2.0")                 |
| id        | string | Request identifier matching the request           |
| result    | object | Block object, or null if the block does not exist |

### Result Object

| Field        | Type   | Description                                        |
| ------------ | ------ | -------------------------------------------------- |
| number       | string | Block number, hex-encoded                          |
| hash         | string | Block hash                                         |
| parentHash   | string | Hash of the parent block                           |
| timestamp    | string | Unix timestamp of the block, hex-encoded           |
| transactions | array  | Transaction hashes, or full objects when requested |

## Use Cases

* **Hash Lookups**: Fetch a block when only its hash is known
* **Reorg Handling**: Confirm a block hash is still on the canonical chain
* **Explorer Links**: Resolve a block-hash link to block detail
* **Verification**: Confirm a block's contents against its hash

## Error Handling

| Error Code | Message        | Description                                           |
| ---------- | -------------- | ----------------------------------------------------- |
| -32602     | Invalid params | The block number or hash is malformed or out of range |
| -32603     | Internal error | The node failed to read the block                     |

## Web3 Integration

{% tabs %}
{% tab title="Ethers.js" %}
{% code title="ethers-example.js" overflow="wrap" %}
```javascript
const { ethers } = require('ethers');

const provider = new ethers.JsonRpcProvider('https://go.getblock.io/<ACCESS-TOKEN>/');

const block = await provider.getBlock('0x8e3f5b2a9c1d4e7f0a6b3c8d5e2f9a1b4c7d0e3f6a9b2c5d8e1f4a7b0c3d6e9f');
console.log(block);
```
{% endcode %}
{% endtab %}

{% tab title="Viem" %}
{% code title="viem-example.js" %}
```javascript
import { createPublicClient, http } from 'viem';
import { sonic } from 'viem/chains';

const client = createPublicClient({ chain: sonic, transport: http('https://go.getblock.io/<ACCESS-TOKEN>/') });

const block = await client.getBlock({ blockHash: '0x8e3f5b2a9c1d4e7f0a6b3c8d5e2f9a1b4c7d0e3f6a9b2c5d8e1f4a7b0c3d6e9f' });
console.log(block);
```
{% endcode %}
{% endtab %}
{% endtabs %}
