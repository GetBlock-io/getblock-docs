---
description: >-
  Example code for the get_transaction_proof JSON-RPC method. Complete guide on
  how to use get_transaction_proof JSON-RPC in GetBlock Web3 documentation.
---

# get\_transaction\_proof - Nervos

Returns a Merkle proof that one or more transactions were included in a block. Used by light clients and cross-chain bridges to verify transaction inclusion without trusting the responder.

## Parameters

| Parameter   | Type          | Required | Description                                                                               |
| ----------- | ------------- | -------- | ----------------------------------------------------------------------------------------- |
| tx\_hashes  | array of H256 | Yes      | Transaction hashes to prove                                                               |
| block\_hash | H256          | No       | Optional block hash; if omitted, the node finds the block containing all the transactions |

## Request Example

{% tabs %}
{% tab title="cURL" %}
```bash
curl --location --request POST 'https://go.getblock.io/<ACCESS-TOKEN>/' \
--header 'Content-Type: application/json' \
--data-raw '{
    "jsonrpc": "2.0",
    "method": "get_transaction_proof",
    "params": [
        [
            "0xa0ef4eb5f4ceeb08a4c8524d84c5da95dce2f608e0ca2ec8091191b0f330c6e0"
        ]
    ],
    "id": "getblock.io"
}'
```
{% endtab %}

{% tab title="JavaScript (Axios)" %}
```javascript
import axios from 'axios';

const data = JSON.stringify({
    "jsonrpc": "2.0",
    "method": "get_transaction_proof",
    "params": [
        [
            "0xa0ef4eb5f4ceeb08a4c8524d84c5da95dce2f608e0ca2ec8091191b0f330c6e0"
        ]
    ],
    "id": "getblock.io"
});

const config = {
    method: 'post',
    url: 'https://go.getblock.io/<ACCESS-TOKEN>/',
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

url = "https://go.getblock.io/<ACCESS-TOKEN>/"

payload = json.dumps({
    "jsonrpc": "2.0",
    "method": "get_transaction_proof",
    "params": [
        [
            "0xa0ef4eb5f4ceeb08a4c8524d84c5da95dce2f608e0ca2ec8091191b0f330c6e0"
        ]
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
        "method": "get_transaction_proof",
        "params": [
                [
                        "0xa0ef4eb5f4ceeb08a4c8524d84c5da95dce2f608e0ca2ec8091191b0f330c6e0"
                ]
        ],
        "id": "getblock.io"
});

    let response = client
        .post("https://go.getblock.io/<ACCESS-TOKEN>/")
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
        "block_hash": "0x02530b25ad0ff677acc365cb73de3e8cc09c7ddd58272e879252e199d08df83b",
        "witnesses_root": "0x6a7e8d3f1c4b9a2e5d8b7c4f9a3e6b1d5c8a2f4e7b1d6c9a3e5b8d2f4a6c8e0b",
        "proof": {
            "indices": [
                "0x0"
            ],
            "lemmas": [
                "0x4a3e8d3f1c4b9a2e5d8b7c4f9a3e6b1d5c8a2f4e7b1d6c9a3e5b8d2f4a6c8e0b"
            ]
        }
    }
}
```
{% endcode %}

## Response Parameters

| Field                  | Type            | Description                                               |
| ---------------------- | --------------- | --------------------------------------------------------- |
| result.block\_hash     | H256            | Block hash where the transactions are included            |
| result.witnesses\_root | H256            | Root of the witnesses Merkle tree                         |
| result.proof.indices   | array of Uint32 | Indices of the transactions being proven within the block |
| result.proof.lemmas    | array of H256   | Merkle proof lemmas needed to reconstruct the root        |

## Use Cases

* Light client transaction-inclusion proofs
* Cross-chain bridges proving CKB transactions on other chains
* SPV-style verification in mobile wallets

## Error Handling

| Status Code | Error Message     | Cause                                                |
| ----------- | ----------------- | ---------------------------------------------------- |
| 403         | Forbidden         | Missing or invalid `<ACCESS-TOKEN>`                  |
| -32602      | Invalid params    | Request parameters are missing or malformed          |
| -32601      | Method not found  | Method does not exist or is not enabled on this node |
| -32603      | Internal error    | Server-side error while processing the request       |
| 429         | Too Many Requests | Rate limit exceeded for your plan                    |

## SDK Integration

{% tabs %}
{% tab title="@ckb-lumos/lumos" %}
```javascript
import { RPC } from '@ckb-lumos/rpc';

const rpc = new RPC('https://go.getblock.io/<ACCESS-TOKEN>/');

// Lumos provides typed methods on `rpc` (e.g. rpc.getTipHeader(), rpc.getBlock(hash)).
// For raw access to any CKB RPC method:
const result = await rpc.send('get_transaction_proof', [["0xa0ef4eb5f4ceeb08a4c8524d84c5da95dce2f608e0ca2ec8091191b0f330c6e0"]]);
console.log(result);
```
{% endtab %}

{% tab title="ckb-sdk-python / requests" %}
```python
# Using the official CKB SDK Python (ckb-sdk-python).
# For methods not yet wrapped, fall back to raw requests.
import requests

response = requests.post('https://go.getblock.io/<ACCESS-TOKEN>/', json={
    'jsonrpc': '2.0',
    'id': 'getblock.io',
    'method': 'get_transaction_proof',
    'params': [["0xa0ef4eb5f4ceeb08a4c8524d84c5da95dce2f608e0ca2ec8091191b0f330c6e0"]]
}})
print(response.json())
```
{% endtab %}
{% endtabs %}
