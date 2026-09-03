---
description: >-
  Example code for the starknet_getBlockWithTxHashes JSON-RPC method. Complete
  guide on how to use starknet_getBlockWithTxHashes JSON-RPC in GetBlock Web3
  documentation.
---

# starknet\_getBlockWithTxHashes - STRK

This method returns a block's header fields together with the list of transaction hashes it contains, given a block\_id. It is a lighter alternative to fetching full transactions.

## Parameters

| Parameter | Type   | Required | Description |
| --------- | ------ | -------- | ----------- |
| block\_id | object | string   | Yes         |

## Request

{% tabs %}
{% tab title="cURL" %}
{% code overflow="wrap" %}
```bash
curl --location --request POST 'https://shared.eu-central-1.getblock.io/<ACCESS-TOKEN>/' \
--header 'Content-Type: application/json' \
--data-raw '{
    "jsonrpc": "2.0",
    "method": "starknet_getBlockWithTxHashes",
    "params": [{ "block_number": 700000 }],
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
    method: 'starknet_getBlockWithTxHashes',
    params: [{ block_number: 700000 }],
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
        'method': 'starknet_getBlockWithTxHashes',
        'params': [{ 'block_number': 700000 }],
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
            "method": "starknet_getBlockWithTxHashes",
            "params": [{ "block_number": 700000 }],
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
        "status": "ACCEPTED_ON_L1",
        "block_hash": "0x041b10c45dc3f39372f7b9409261cac9d880c5d75a5bb077d028db20b1bd76c4",
        "block_number": 700000,
        "parent_hash": "0x03b6d94b246815960f38b7dfc8fd6df32b1eaf4c3ee1a1b2c9d0e5f6a7b8c9d0",
        "new_root": "0x0525f...",
        "timestamp": 1710000000,
        "sequencer_address": "0x07c57808b9cea7130c44aab2f8ca6147b04408943b48c6d8c3c83eb8cfdd8c0b",
        "l1_gas_price": {
            "price_in_wei": "0x3b9aca00",
            "price_in_fri": "0x5af3107a4000"
        },
        "starknet_version": "0.13.2",
        "transactions": [
            "0x5fb5b63f0226ef426c81168d0235269398b63aa145ca6a3c47294caa691cfdc",
            "0x1c2f...",
            "0x8d3a..."
        ]
    }
}
```

## Response Parameters

| Parameter | Type   | Description                                             |
| --------- | ------ | ------------------------------------------------------- |
| jsonrpc   | string | JSON-RPC protocol version ("2.0")                       |
| id        | string | Request identifier matching the request                 |
| result    | object | Block header plus transaction hashes (see fields below) |

### Result Object

| Field          | Type   | Description                                                  |
| -------------- | ------ | ------------------------------------------------------------ |
| status         | string | Block acceptance status (ACCEPTED\_ON\_L2, ACCEPTED\_ON\_L1) |
| block\_hash    | string | Hash of the block                                            |
| l1\_gas\_price | object | L1 gas price in wei (ETH) and fri (STRK)                     |
| transactions   | array  | Transaction hashes included in the block                     |

## Use Cases

* **Lightweight Indexing**: List block transaction hashes without full bodies
* **Confirmation Status**: Read whether a block is ACCEPTED\_ON\_L1
* **Fee Tracking**: Read l1\_gas\_price per block
* **Explorer Backends**: Render block summaries

## Error Handling

| Error                     | Message         | Description                                       |
| ------------------------- | --------------- | ------------------------------------------------- |
| 24 / BLOCK\_NOT\_FOUND    | Block not found | No block matches the supplied block\_id           |
| 403 / RBAC: access denied | Access denied   | The GetBlock access token is missing or incorrect |
