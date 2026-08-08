---
description: >-
  Example code for the eth_getBlockByNumber JSON-RPC method. Complete guide to
  using the eth_getBlockByNumber JSON-RPC method in the GetBlock.io Web3
  documentation.
---

# eth\_getBlockByNumber - Unichain

This method returns information about a block by its number. The second parameter controls whether full transaction objects or only transaction hashes are returned.

## Parameters

| Parameter        | Type    | Required | Description                                              |
| ---------------- | ------- | -------- | -------------------------------------------------------- |
| blockParameter   | string  | Yes      | Block number in hex, or "latest", "earliest", "pending"  |
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
    "method": "eth_getBlockByNumber",
    "params": ["0x1e8480", false],
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
    method: 'eth_getBlockByNumber',
    params: ["0x1e8480", false],
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
        'method': 'eth_getBlockByNumber',
        'params': ["0x1e8480", false],
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
            "method": "eth_getBlockByNumber",
            "params": ["0x1e8480", false],
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
        "miner": "0x0000000000000000000000000000000000000000",
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

| Field         | Type   | Description                                        |
| ------------- | ------ | -------------------------------------------------- |
| number        | string | Block number, hex-encoded                          |
| hash          | string | Block hash                                         |
| parentHash    | string | Hash of the parent block                           |
| timestamp     | string | Unix timestamp of the block, hex-encoded           |
| gasUsed       | string | Total gas used by the block                        |
| baseFeePerGas | string | Base fee per gas of the block                      |
| transactions  | array  | Transaction hashes, or full objects when requested |

## Use Cases

* **Block Explorers**: Render block metadata and its transaction list
* **Chain Indexing**: Stream blocks by number into an off-chain store
* **Timestamp Mapping**: Read a block's timestamp for time-based logic
* **Fee Analysis**: Read base fee and gas used per block

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

const provider = new ethers.JsonRpcProvider('https://go.getblock.io/<ACCESS-TOKEN>/');

const block = await provider.getBlock(2000000);
console.log(block);
```
{% endcode %}
{% endtab %}

{% tab title="Viem" %}
{% code title="viem-example.js" %}
```javascript
import { createPublicClient, http } from 'viem';
import { unichain } from 'viem/chains';

const client = createPublicClient({ chain: unichain, transport: http('https://go.getblock.io/<ACCESS-TOKEN>/') });

const block = await client.getBlock({ blockNumber: 2000000n });
console.log(block);
```
{% endcode %}
{% endtab %}
{% endtabs %}
