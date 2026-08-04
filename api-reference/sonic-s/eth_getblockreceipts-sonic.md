---
description: >-
  Example code for the eth_getBlockReceipts JSON-RPC method. Complete guide on
  how to use eth_getBlockReceipts JSON-RPC in GetBlock Web3 documentation.
---

# eth\_getBlockReceipts - Sonic

This method returns all transaction receipts for a block in a single call. It is used to read every transaction outcome and log in a block without one call per transaction.

## Parameters

| Parameter      | Type   | Required | Description                                                         |
| -------------- | ------ | -------- | ------------------------------------------------------------------- |
| blockParameter | string | Yes      | Block number in hex, block hash, or "latest", "earliest", "pending" |

## Request

{% tabs %}
{% tab title="cURL" %}
{% code overflow="wrap" %}
```bash
curl --location --request POST 'https://go.getblock.io/<ACCESS-TOKEN>/' \
--header 'Content-Type: application/json' \
--data-raw '{
    "jsonrpc": "2.0",
    "method": "eth_getBlockReceipts",
    "params": ["0x1e8480"],
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
    method: 'eth_getBlockReceipts',
    params: ["0x1e8480"],
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
        'method': 'eth_getBlockReceipts',
        'params': ["0x1e8480"],
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
            "method": "eth_getBlockReceipts",
            "params": ["0x1e8480"],
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
            "transactionHash": "0x4a7b0c3d6e9f2a5b8c1d4e7f0a3b6c9d2e5f8a1b4c7d0e3f6a9b2c5d8e1f4a7b",
            "transactionIndex": "0x0",
            "blockHash": "0x8e3f5b2a9c1d4e7f0a6b3c8d5e2f9a1b4c7d0e3f6a9b2c5d8e1f4a7b0c3d6e9f",
            "blockNumber": "0x1e8480",
            "from": "0xd8dA6BF26964aF9D7eEd9e03E53415D37aA96045",
            "to": "0x039e2fB66102314Ce7b64Ce5Ce3E5183bc94aD38",
            "gasUsed": "0x5208",
            "cumulativeGasUsed": "0x5208",
            "status": "0x1",
            "logs": []
        }
    ]
}
```

## Response Parameters

| Parameter | Type   | Description                                        |
| --------- | ------ | -------------------------------------------------- |
| jsonrpc   | string | JSON-RPC protocol version ("2.0")                  |
| id        | string | Request identifier matching the request            |
| result    | array  | Array of transaction receipt objects for the block |

### Receipt Object

| Field           | Type   | Description                           |
| --------------- | ------ | ------------------------------------- |
| transactionHash | string | Hash of the transaction               |
| status          | string | 0x1 on success, 0x0 on failure        |
| gasUsed         | string | Gas used by the transaction           |
| logs            | array  | Event logs emitted by the transaction |

## Use Cases

* **Block Indexing**: Read all outcomes and logs for a block in one call
* **Event Pipelines**: Ingest every log in a block efficiently
* **Reconciliation**: Confirm the status of every transaction in a block
* **Analytics**: Aggregate gas usage across a block's transactions

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

const receipts = await provider.send('eth_getBlockReceipts', ['0x1e8480']);
console.log(receipts);
```
{% endcode %}
{% endtab %}

{% tab title="Viem" %}
{% code title="viem-example.js" %}
```javascript
import { createPublicClient, http } from 'viem';
import { sonic } from 'viem/chains';

const client = createPublicClient({ chain: sonic, transport: http('https://go.getblock.io/<ACCESS-TOKEN>/') });

const receipts = await client.getBlockReceipts({ blockNumber: 2000000n });
console.log(receipts);
```
{% endcode %}
{% endtab %}
{% endtabs %}
