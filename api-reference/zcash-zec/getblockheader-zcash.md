---
description: >-
  Example code for the getblockheader JSON-RPC method. Сomplete guide on how to
  use the getblockheader JSON-RPC method in GetBlock.io Web3 documentation.
---

# getblockheader - Zcash

This method returns block header data — the fixed 80-byte header plus derived fields — without the full transaction list. Significantly cheaper than `getblock` when only header data is needed, such as during header-first chain synchronization or SPV-style verification.

## Parameters

| Parameter   | Type    | Required | Description                                                                            |
| ----------- | ------- | -------- | -------------------------------------------------------------------------------------- |
| `blockhash` | string  | Yes      | Block hash (hex string)                                                                |
| `verbose`   | boolean | No       | If `true` (default), returns a JSON object; if `false`, returns raw hex-encoded header |

## Request

{% tabs %}
{% tab title="cURL" %}
{% code overflow="wrap" %}
```bash
curl --location --request POST 'https://shared.eu-central-1.getblock.io/<ACCESS-TOKEN>/' \
--header 'Content-Type: application/json' \
--data-raw '{
    "jsonrpc": "2.0",
    "method": "getblockheader",
    "params": [
        "00000000018bb43cadf5b4c8a2e7f9d0e1a6b3c8d5e4f2a1b0c9d8e7f6a5b4c3",
        true
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
    method: 'getblockheader',
    params: [
        "00000000018bb43cadf5b4c8a2e7f9d0e1a6b3c8d5e4f2a1b0c9d8e7f6a5b4c3",
        true
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
        'method': 'getblockheader',
        'params': [
        "00000000018bb43cadf5b4c8a2e7f9d0e1a6b3c8d5e4f2a1b0c9d8e7f6a5b4c3",
        true
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
            "method": "getblockheader",
            "params": [
        "00000000018bb43cadf5b4c8a2e7f9d0e1a6b3c8d5e4f2a1b0c9d8e7f6a5b4c3",
        true
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

{% code overflow="wrap" %}
```json
{
    "jsonrpc": "2.0",
    "id": "getblock.io",
    "result": {
        "hash": "00000000018bb43cadf5b4c8a2e7f9d0e1a6b3c8d5e4f2a1b0c9d8e7f6a5b4c3",
        "confirmations": 1,
        "height": 2856342,
        "version": 4,
        "merkleroot": "3a2b1c9d8e7f6a5b4c3d2e1f0a9b8c7d6e5f4a3b2c1d0e9f8a7b6c5d4e3f2a1b",
        "blockcommitments": "5eae9c5cbe0e8d3e2f5cd8b0a4e3b1f2c9d8e7f6a5b4c3d2e1f0a9b8c7d6e5f4",
        "finalsaplingroot": "01a1b2c3d4e5f6a7b8c9d0e1f2a3b4c5d6e7f8a9b0c1d2e3f4a5b6c7d8e9f0a1",
        "time": 1747852800,
        "nonce": "0000000000000000000000000000000000000000000000000000000000abcdef",
        "solution": "008f2c6b5a1e2d4f9c...",
        "bits": "1d00ffff",
        "difficulty": 78421563.42,
        "previousblockhash": "00000000019cc54dbe07a2f9d0e1a6b3c8d5e4f2a1b0c9d8e7f6a5b4c3d2e1f0",
        "nextblockhash": ""
    }
}
```
{% endcode %}

## Response Parameters

| Parameter           | Type    | Description                                          |
| ------------------- | ------- | ---------------------------------------------------- |
| `hash`              | string  | Block hash                                           |
| `confirmations`     | integer | Number of confirmations                              |
| `height`            | integer | Block height                                         |
| `version`           | integer | Block version                                        |
| `merkleroot`        | string  | Merkle root of transactions                          |
| `blockcommitments`  | string  | Block commitments hash (post-Heartwood)              |
| `finalsaplingroot`  | string  | Final Sapling commitment tree state after this block |
| `time`              | integer | Unix timestamp                                       |
| `nonce`             | string  | Block nonce (hex)                                    |
| `solution`          | string  | Equihash solution (hex)                              |
| `bits`              | string  | Compact difficulty target                            |
| `difficulty`        | number  | Difficulty of this block                             |
| `previousblockhash` | string  | Parent block hash                                    |
| `nextblockhash`     | string  | Next block hash (empty at chain tip)                 |

## Use Cases

* **Header-First Sync**: SPV clients download headers first to establish the canonical chain before fetching transactions
* **Reorg Detection**: Compare header hashes across polls to detect chain reorganization
* **Lightweight Block Metadata**: Get block timing, difficulty, and shielded-tree state without loading transactions
* **Chain History Reconstruction**: Iterate headers backwards from a known hash to reconstruct chain history

## Error Handling

| Error Code | Message         | Description                        |
| ---------- | --------------- | ---------------------------------- |
| -5         | Block not found | Requested hash doesn't exist       |
| -32602     | Invalid params  | Hash is malformed                  |
| -32603     | Internal error  | Node failed to retrieve the header |
