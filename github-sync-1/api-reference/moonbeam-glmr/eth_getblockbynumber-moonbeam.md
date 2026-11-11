---
description: >-
  Example code for the eth_getBlockByNumber JSON-RPC method. Complete guide on
  how to use eth_getBlockByNumber JSON-RPC in GetBlock Web3 documentation.
---

# eth\_getBlockByNumber - Moonbeam

This method returns block data for a hex-encoded block number or a tag such as latest, safe, finalized, earliest, or pending. It is the most common block-lookup method for indexers and explorers on Moonbeam.

## Parameters

| Parameter        | Type    | Required | Description                                                                  |
| ---------------- | ------- | -------- | ---------------------------------------------------------------------------- |
| blockParameter   | string  | Yes      | Block number in hex, or "latest", "earliest", "pending", "safe", "finalized" |
| fullTransactions | boolean | Yes      | true for full transaction objects, false for hashes only                     |

## Request

{% tabs %}
{% tab title="cURL" %}
{% code overflow="wrap" %}
```bash
curl --location --request POST 'https://shared.eu-central-1.getblock.io/<ACCESS-TOKEN>/' \
--header 'Content-Type: application/json' \
--data-raw '{
    "jsonrpc": "2.0",
    "method": "eth_getBlockByNumber",
    "params": ["latest", false],
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
    method: 'eth_getBlockByNumber',
    params: ['latest', false],
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
        'method': 'eth_getBlockByNumber',
        'params': ['latest', False],
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
            "method": "eth_getBlockByNumber",
            "params": ["latest", false],
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
        "number": "0xb71b00",
        "hash": "0x9f2b4c1d7e3a6084b5c9d0e1f2a3b4c5d6e7f8091a2b3c4d5e6f708192a3b4c5d",
        "parentHash": "0x8a1c...",
        "timestamp": "0x66f0a3c0",
        "gasLimit": "0x3938700",
        "gasUsed": "0x1c9c380",
        "baseFeePerGas": "0x7",
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

* **Chain Following**: Fetch the latest block on a \~1 second cadence via the "latest" tag
* **Finality Checks**: Read the "finalized" or "safe" block for settlement-grade reads
* **Historical Queries**: Read any past block by passing its hex height
* **Pending Preview**: Inspect the "pending" block for not-yet-mined transactions

## Error Handling

| Error Code | Message        | Description                                        |
| ---------- | -------------- | -------------------------------------------------- |
| -32602     | Invalid params | Malformed block number/tag or missing boolean flag |
| -32603     | Internal error | The node failed to read the block                  |

## Web3 Integration

{% tabs %}
{% tab title="Ethers.js" %}
{% code title="ethers-example.js" %}
```javascript
import { ethers } from 'ethers';

const provider = new ethers.JsonRpcProvider('https://shared.eu-central-1.getblock.io/<ACCESS-TOKEN>/');

const block = await provider.getBlock('latest');
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

const block = await client.getBlock({ blockTag: 'latest' });
```
{% endcode %}
{% endtab %}
{% endtabs %}
