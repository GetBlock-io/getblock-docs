---
description: >-
  Example code for the eth_getBlockByHash JSON-RPC method. Complete guide on how
  to use eth_getBlockByHash JSON-RPC in GetBlock Web3 documentation.
---

# eth\_getBlockByHash - Arbitrum Nova

This method returns a block's data given its 32-byte hash. A boolean flag controls whether full transaction objects or only transaction hashes are returned.

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
curl --location --request POST 'https://shared.eu-central-1.getblock.io/<ACCESS-TOKEN>/' \
--header 'Content-Type: application/json' \
--data-raw '{
    "jsonrpc": "2.0",
    "method": "eth_getBlockByHash",
    "params": ["0x9f2b4c1d7e3a6084b5c9d0e1f2a3b4c5d6e7f8091a2b3c4d5e6f708192a3b4c5d", false],
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
    method: 'eth_getBlockByHash',
    params: ['0x9f2b4c1d7e3a6084b5c9d0e1f2a3b4c5d6e7f8091a2b3c4d5e6f708192a3b4c5d', false],
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
        'method': 'eth_getBlockByHash',
        'params': ['0x9f2b4c1d7e3a6084b5c9d0e1f2a3b4c5d6e7f8091a2b3c4d5e6f708192a3b4c5d', False],
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
            "method": "eth_getBlockByHash",
            "params": ["0x9f2b4c1d7e3a6084b5c9d0e1f2a3b4c5d6e7f8091a2b3c4d5e6f708192a3b4c5d", false],
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
        "number": "0x14b8a10",
        "hash": "0x9f2b4c1d7e3a6084b5c9d0e1f2a3b4c5d6e7f8091a2b3c4d5e6f708192a3b4c5d",
        "parentHash": "0x8a1c...",
        "timestamp": "0x66f0a3c0",
        "gasLimit": "0x3938700",
        "gasUsed": "0x1c9c380",
        "miner": "0x4200000000000000000000000000000000000011",
        "transactions": [
            "0x3c8a1f5b2d9e4076c1a8b3d5e7f9021436587a9cbdef012345678abcdef901234"
        ]
    }
}
```

## Response Parameters

| Parameter | Type   | Description                                         |
| --------- | ------ | --------------------------------------------------- |
| jsonrpc   | string | JSON-RPC protocol version ("2.0")                   |
| id        | string | Request identifier matching the request             |
| result    | object | Block header and transactions, or null if not found |

## Use Cases

* **Block Inspection**: Read header fields and contents for a known block hash
* **Reorg Detection**: Compare a stored block hash against the canonical chain
* **Explorer Backends**: Render block detail pages from hash lookups
* **Data Integrity**: Confirm a block hash resolves to the expected header

## Error Handling

| Error Code | Message        | Description                                  |
| ---------- | -------------- | -------------------------------------------- |
| -32602     | Invalid params | Malformed block hash or missing boolean flag |
| -32603     | Internal error | The node failed to read the block            |

## Web3 Integration

{% tabs %}
{% tab title="Ethers.js" %}
{% code title="ethers-example.js" %}
```javascript
import { ethers } from 'ethers';

const provider = new ethers.JsonRpcProvider('https://shared.eu-central-1.getblock.io/<ACCESS-TOKEN>/');

const block = await provider.getBlock('0x9f2b4c1d7e3a6084b5c9d0e1f2a3b4c5d6e7f8091a2b3c4d5e6f708192a3b4c5d');
```
{% endcode %}
{% endtab %}

{% tab title="Viem" %}
{% code title="viem-example.js" %}
```javascript
import { createPublicClient, http } from 'viem';

const client = createPublicClient({
  transport: http('https://shared.eu-central-1.getblock.io/<ACCESS-TOKEN>/')
});

const block = await client.getBlock({ blockHash: '0x9f2b4c1d7e3a6084b5c9d0e1f2a3b4c5d6e7f8091a2b3c4d5e6f708192a3b4c5d' });
```
{% endcode %}
{% endtab %}
{% endtabs %}
