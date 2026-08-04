---
description: >-
  Example code for the eth_getTransactionByBlockHashAndIndex JSON-RPC method.
  Complete guide on how to use eth_getTransactionByBlockHashAndIndex JSON-RPC in
  GetBlock Web3 documentation.
---

# eth\_getTransactionByBlockHashAndIndex - Linea

This method returns a transaction by its position in a block identified by hash. The index is the transaction's zero-based position within the block.

## Parameters

| Parameter | Type   | Required | Description                             |
| --------- | ------ | -------- | --------------------------------------- |
| blockHash | string | Yes      | 32-byte block hash                      |
| index     | string | Yes      | Transaction index position, hex-encoded |

## Request

{% tabs %}
{% tab title="cURL" %}
{% code overflow="wrap" %}
```bash
curl --location --request POST 'https://go.getblock.io/<ACCESS-TOKEN>/' \
--header 'Content-Type: application/json' \
--data-raw '{
    "jsonrpc": "2.0",
    "method": "eth_getTransactionByBlockHashAndIndex",
    "params": ["0x8e3f5b2a9c1d4e7f0a6b3c8d5e2f9a1b4c7d0e3f6a9b2c5d8e1f4a7b0c3d6e9f", "0x0"],
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
    method: 'eth_getTransactionByBlockHashAndIndex',
    params: ["0x8e3f5b2a9c1d4e7f0a6b3c8d5e2f9a1b4c7d0e3f6a9b2c5d8e1f4a7b0c3d6e9f", "0x0"],
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
        'method': 'eth_getTransactionByBlockHashAndIndex',
        'params': ["0x8e3f5b2a9c1d4e7f0a6b3c8d5e2f9a1b4c7d0e3f6a9b2c5d8e1f4a7b0c3d6e9f", "0x0"],
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
            "method": "eth_getTransactionByBlockHashAndIndex",
            "params": ["0x8e3f5b2a9c1d4e7f0a6b3c8d5e2f9a1b4c7d0e3f6a9b2c5d8e1f4a7b0c3d6e9f", "0x0"],
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
        "hash": "0x4a7b0c3d6e9f2a5b8c1d4e7f0a3b6c9d2e5f8a1b4c7d0e3f6a9b2c5d8e1f4a7b",
        "blockHash": "0x8e3f5b2a9c1d4e7f0a6b3c8d5e2f9a1b4c7d0e3f6a9b2c5d8e1f4a7b0c3d6e9f",
        "blockNumber": "0x1e8480",
        "transactionIndex": "0x0",
        "from": "0xd8dA6BF26964aF9D7eEd9e03E53415D37aA96045",
        "to": "0xe5D7C2a44FfDDf6b295A15c148167daaAf5Cf34f",
        "value": "0xde0b6b3a7640000",
        "nonce": "0x2a",
        "type": "0x2"
    }
}
```

## Response Parameters

| Parameter | Type   | Description                                                        |
| --------- | ------ | ------------------------------------------------------------------ |
| jsonrpc   | string | JSON-RPC protocol version ("2.0")                                  |
| id        | string | Request identifier matching the request                            |
| result    | object | Transaction object, or null if no transaction exists at that index |

### Result Object

| Field            | Type   | Description                                      |
| ---------------- | ------ | ------------------------------------------------ |
| hash             | string | Transaction hash                                 |
| from             | string | Sender address                                   |
| to               | string | Recipient address, or null for contract creation |
| transactionIndex | string | Index of the transaction within the block        |

## Use Cases

* **Block Iteration**: Walk a block's transactions by index using its hash
* **Ordered Access**: Read a transaction at a known position
* **Reorg-Safe Reads**: Pin reads to a specific block hash
* **Explorer Detail**: Resolve a block-hash-and-index reference to a transaction

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

const tx = await provider.send('eth_getTransactionByBlockHashAndIndex', ['0x8e3f5b2a9c1d4e7f0a6b3c8d5e2f9a1b4c7d0e3f6a9b2c5d8e1f4a7b0c3d6e9f', '0x0']);
console.log(tx);
```
{% endcode %}
{% endtab %}

{% tab title="Viem" %}
{% code title="viem-example.js" %}
```javascript
import { createPublicClient, http } from 'viem';
import { linea } from 'viem/chains';

const client = createPublicClient({ chain: linea, transport: http('https://go.getblock.io/<ACCESS-TOKEN>/') });

const tx = await client.getTransaction({ blockHash: '0x8e3f5b2a9c1d4e7f0a6b3c8d5e2f9a1b4c7d0e3f6a9b2c5d8e1f4a7b0c3d6e9f', index: 0 });
console.log(tx);
```
{% endcode %}
{% endtab %}
{% endtabs %}
