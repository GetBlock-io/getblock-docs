---
description: >-
  Example code for the eth_getBlockByHash JSON-RPC method. Complete guide to
  using the eth_getBlockByHash JSON-RPC method in GetBlock.io Web3
  documentation.
---

# eth\_getBlockByHash - Robinhood

Returns block information by hash. Pass `true` as the second parameter to include full transaction objects; `false` to receive only transaction hashes. Robinhood Chain blocks may include an additional `l1BlockNumber` field indicating the L1 block that included the batch containing this L2 block.

## Parameters

| Parameter        | Type            | Required | Description                                                  |
| ---------------- | --------------- | -------- | ------------------------------------------------------------ |
| blockHash        | DATA (32 bytes) | Yes      | Block hash                                                   |
| fullTransactions | boolean         | Yes      | `true` for full transaction objects, `false` for hashes only |

## Request Example

{% tabs %}
{% tab title="cURL" %}
{% code overflow="wrap" %}
```bash
curl --location --request POST 'https://shared.us-east-1.getblock.io/<ACCESS-TOKEN>/' \
--header 'Content-Type: application/json' \
--data-raw '{
    "jsonrpc": "2.0",
    "method": "eth_getBlockByHash",
    "params": [
        "0x8b5c6f1a9e8d7c6b5a4f3e2d1c0b9a8e7f6d5c4b3a2b1f9e8d7c6b5a4f3e2d1c",
        false
    ],
    "id": "getblock.io"
}'
```
{% endcode %}
{% endtab %}

{% tab title="JavaScript (Axios)" %}
```javascript
import axios from 'axios';

const data = JSON.stringify({
    "jsonrpc": "2.0",
    "method": "eth_getBlockByHash",
    "params": [
        "0x8b5c6f1a9e8d7c6b5a4f3e2d1c0b9a8e7f6d5c4b3a2b1f9e8d7c6b5a4f3e2d1c",
        false
    ],
    "id": "getblock.io"
});

const config = {
    method: 'post',
    url: 'https://shared.us-east-1.getblock.io/<ACCESS-TOKEN>/',
    headers: {
        'Content-Type': 'application/json'
    },
    data: data
};

axios(config)
    .then(response => console.log(JSON.stringify(response.data, null, 2)))
    .catch(error => console.log(error));
```
{% endtab %}

{% tab title="Python (Requests)" %}
```python
import requests
import json

url = "https://shared.us-east-1.getblock.io/<ACCESS-TOKEN>/"

payload = json.dumps({
    "jsonrpc": "2.0",
    "method": "eth_getBlockByHash",
    "params": [
        "0x8b5c6f1a9e8d7c6b5a4f3e2d1c0b9a8e7f6d5c4b3a2b1f9e8d7c6b5a4f3e2d1c",
        false
    ],
    "id": "getblock.io"
})

headers = {
    'Content-Type': 'application/json'
}

response = requests.post(url, headers=headers, data=payload)
print(response.text)
```
{% endtab %}

{% tab title="Rust (Reqwest)" %}
```rust
use reqwest::Client;
use serde_json::json;

#[tokio::main]
async fn main() -> Result<(), Box<dyn std::error::Error>> {
    let client = Client::new();

    let payload = json!({
        "jsonrpc": "2.0",
        "method": "eth_getBlockByHash",
        "params": [
                "0x8b5c6f1a9e8d7c6b5a4f3e2d1c0b9a8e7f6d5c4b3a2b1f9e8d7c6b5a4f3e2d1c",
                false
        ],
        "id": "getblock.io"
});

    let response = client
        .post("https://shared.us-east-1.getblock.io/<ACCESS-TOKEN>/")
        .header("Content-Type", "application/json")
        .json(&payload)
        .send()
        .await?;

    let result: serde_json::Value = response.json().await?;
    println!("{:#?}", result);

    Ok(())
}
```
{% endtab %}
{% endtabs %}

## Response Example

{% code overflow="wrap" %}
```json
{
    "jsonrpc": "2.0",
    "id": "getblock.io",
    "result": {
        "number": "0x124f80",
        "hash": "0x8b5c6f1a9e8d7c6b5a4f3e2d1c0b9a8e7f6d5c4b3a2b1f9e8d7c6b5a4f3e2d1c",
        "parentHash": "0x47edbdc8da5cd1b3127ea8f1ce5da6739fa39e7e6d2eb1c9b50f1c83de0a98b4",
        "timestamp": "0x68f3b85f",
        "miner": "0xa4b000000000000000000073657175656e636572",
        "gasLimit": "0x1c9c380",
        "gasUsed": "0xe4e1c0",
        "baseFeePerGas": "0x5f5e100",
        "l1BlockNumber": "0x1234567",
        "transactions": [
            "0xa8f9b3c7d2e4f6a1b9c8d7e6f5a4b3c2d1e0f9a8b7c6d5e4f3a2b1c0d9e8f7a6"
        ],
        "difficulty": "0x1",
        "totalDifficulty": "0x1",
        "size": "0x42a",
        "nonce": "0x0000000000000000"
    }
}
```
{% endcode %}

## Response Parameters

<table data-search="false"><thead><tr><th>Field</th><th>Type</th><th>Description</th></tr></thead><tbody><tr><td>result.number</td><td>QUANTITY</td><td>Block number</td></tr><tr><td>result.hash</td><td>DATA</td><td>Block hash</td></tr><tr><td>result.parentHash</td><td>DATA</td><td>Parent block hash</td></tr><tr><td>result.timestamp</td><td>QUANTITY</td><td>Unix timestamp of block creation</td></tr><tr><td>result.miner</td><td>DATA</td><td>Address that produced the block (sequencer address on Arbitrum Orbit)</td></tr><tr><td>result.gasLimit</td><td>QUANTITY</td><td>Maximum gas allowed in the block</td></tr><tr><td>result.gasUsed</td><td>QUANTITY</td><td>Total gas used by all transactions</td></tr><tr><td>result.baseFeePerGas</td><td>QUANTITY</td><td>EIP-1559 base fee</td></tr><tr><td>result.l1BlockNumber</td><td>QUANTITY</td><td>Arbitrum-specific: L1 block that included the batch containing this L2 block</td></tr><tr><td>result.transactions</td><td>array</td><td>Transaction hashes (or full objects if requested)</td></tr></tbody></table>

## Use Cases

* Block-by-block indexing in explorers
* Verifying that a specific block exists and is canonical
* Tracking L1 anchoring via the `l1BlockNumber` field

## Error Handling

| Status Code | Error Message     | Cause                                          |
| ----------- | ----------------- | ---------------------------------------------- |
| 403         | Forbidden         | Missing or invalid `<ACCESS-TOKEN>`            |
| -32602      | Invalid params    | Request parameters are missing or malformed    |
| -32603      | Internal error    | Server-side error while processing the request |
| 429         | Too Many Requests | Rate limit exceeded for your plan              |

## SDK Integration

{% tabs %}
{% tab title="ethers.js" %}
{% code overflow="wrap" %}
```javascript
import { ethers } from 'ethers';

const provider = new ethers.JsonRpcProvider('https://shared.us-east-1.getblock.io/<ACCESS-TOKEN>/');

// Generic JSON-RPC call (works for any method):
const result = await provider.send('eth_getBlockByHash', ["0x8b5c6f1a9e8d7c6b5a4f3e2d1c0b9a8e7f6d5c4b3a2b1f9e8d7c6b5a4f3e2d1c", false]);
console.log(result);

// Many standard methods have typed wrappers on ethers Provider,
// e.g. provider.getBalance(addr), provider.getBlock(n), provider.getTransaction(hash).
```
{% endcode %}
{% endtab %}

{% tab title="viem" %}
{% code overflow="wrap" %}
```javascript
import { createPublicClient, http } from 'viem';

const client = createPublicClient({
    transport: http('https://shared.us-east-1.getblock.io/<ACCESS-TOKEN>/')
});

// Generic JSON-RPC request (works for any method):
const result = await client.request({
    method: 'eth_getBlockByHash',
    params: ["0x8b5c6f1a9e8d7c6b5a4f3e2d1c0b9a8e7f6d5c4b3a2b1f9e8d7c6b5a4f3e2d1c", false]
});
console.log(result);

// Many standard methods have typed wrappers on PublicClient,
// e.g. client.getBalance({ address }), client.getBlock({ blockNumber }).
```
{% endcode %}
{% endtab %}
{% endtabs %}
