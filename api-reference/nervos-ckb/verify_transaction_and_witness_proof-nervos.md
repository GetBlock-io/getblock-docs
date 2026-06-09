---
description: >-
  Example code for the verify_transaction_and_witness_proof JSON-RPC method.
  Complete guide on how to use verify_transaction_and_witness_proof JSON-RPC in
  GetBlock Web3 documentation.
---

# verify\_transaction\_and\_witness\_proof - Nervos

Verifies a transaction-and-witness proof and returns the transaction hashes it witnesses.

## Parameters

| Parameter | Type                       | Required | Description                                                  |
| --------- | -------------------------- | -------- | ------------------------------------------------------------ |
| tx\_proof | TransactionAndWitnessProof | Yes      | Proof object returned by `get_transaction_and_witness_proof` |

## Request Example

{% tabs %}
{% tab title="cURL" %}
```bash
curl --location --request POST 'https://go.getblock.io/<ACCESS-TOKEN>/' \
--header 'Content-Type: application/json' \
--data-raw '{
    "jsonrpc": "2.0",
    "method": "verify_transaction_and_witness_proof",
    "params": [
        {
            "block_hash": "0x02530b25ad0ff677acc365cb73de3e8cc09c7ddd58272e879252e199d08df83b",
            "transactions_proof": {
                "indices": [
                    "0x0"
                ],
                "lemmas": [
                    "0x4a3e8d3f1c4b9a2e5d8b7c4f9a3e6b1d5c8a2f4e7b1d6c9a3e5b8d2f4a6c8e0b"
                ]
            },
            "witnesses_proof": {
                "indices": [
                    "0x0"
                ],
                "lemmas": [
                    "0x8a7e3d3f1c4b9a2e5d8b7c4f9a3e6b1d5c8a2f4e7b1d6c9a3e5b8d2f4a6c8e0b"
                ]
            }
        }
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
    "method": "verify_transaction_and_witness_proof",
    "params": [
        {
            "block_hash": "0x02530b25ad0ff677acc365cb73de3e8cc09c7ddd58272e879252e199d08df83b",
            "transactions_proof": {
                "indices": [
                    "0x0"
                ],
                "lemmas": [
                    "0x4a3e8d3f1c4b9a2e5d8b7c4f9a3e6b1d5c8a2f4e7b1d6c9a3e5b8d2f4a6c8e0b"
                ]
            },
            "witnesses_proof": {
                "indices": [
                    "0x0"
                ],
                "lemmas": [
                    "0x8a7e3d3f1c4b9a2e5d8b7c4f9a3e6b1d5c8a2f4e7b1d6c9a3e5b8d2f4a6c8e0b"
                ]
            }
        }
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
    "method": "verify_transaction_and_witness_proof",
    "params": [
        {
            "block_hash": "0x02530b25ad0ff677acc365cb73de3e8cc09c7ddd58272e879252e199d08df83b",
            "transactions_proof": {
                "indices": [
                    "0x0"
                ],
                "lemmas": [
                    "0x4a3e8d3f1c4b9a2e5d8b7c4f9a3e6b1d5c8a2f4e7b1d6c9a3e5b8d2f4a6c8e0b"
                ]
            },
            "witnesses_proof": {
                "indices": [
                    "0x0"
                ],
                "lemmas": [
                    "0x8a7e3d3f1c4b9a2e5d8b7c4f9a3e6b1d5c8a2f4e7b1d6c9a3e5b8d2f4a6c8e0b"
                ]
            }
        }
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
        "method": "verify_transaction_and_witness_proof",
        "params": [
                {
                        "block_hash": "0x02530b25ad0ff677acc365cb73de3e8cc09c7ddd58272e879252e199d08df83b",
                        "transactions_proof": {
                                "indices": [
                                        "0x0"
                                ],
                                "lemmas": [
                                        "0x4a3e8d3f1c4b9a2e5d8b7c4f9a3e6b1d5c8a2f4e7b1d6c9a3e5b8d2f4a6c8e0b"
                                ]
                        },
                        "witnesses_proof": {
                                "indices": [
                                        "0x0"
                                ],
                                "lemmas": [
                                        "0x8a7e3d3f1c4b9a2e5d8b7c4f9a3e6b1d5c8a2f4e7b1d6c9a3e5b8d2f4a6c8e0b"
                                ]
                        }
                }
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
    "result": [
        "0xa0ef4eb5f4ceeb08a4c8524d84c5da95dce2f608e0ca2ec8091191b0f330c6e0"
    ]
}
```
{% endcode %}

## Response Parameters

| Field  | Type          | Description                                                           |
| ------ | ------------- | --------------------------------------------------------------------- |
| result | array of H256 | Transaction hashes whose inclusion (and witnesses) the proof verifies |

## Use Cases

* Strong-form light client verification
* Cross-chain proof relayers

## Error Handling

| Status Code | Error Message     | Cause                                                |
| ----------- | ----------------- | ---------------------------------------------------- |
| 403         | Forbidden         | Missing or invalid `<ACCESS-TOKEN>`                  |
| -32602      | Invalid params    | Request parameters are missing or malformed          |
| -32601      | Method not found  | Method does not exist or is not enabled on this node |
| -32603      | Internal error    | Server-side error while processing the request       |
| 429         | Too Many Requests | Rate limit exceeded for your plan                    |
| -32602      | Invalid proof     | Proof is malformed or doesn't verify                 |

## SDK Integration

{% tabs %}
{% tab title="@ckb-lumos/lumos" %}
```javascript
import { RPC } from '@ckb-lumos/rpc';

const rpc = new RPC('https://go.getblock.io/<ACCESS-TOKEN>/');

// Lumos provides typed methods on `rpc` (e.g. rpc.getTipHeader(), rpc.getBlock(hash)).
// For raw access to any CKB RPC method:
const result = await rpc.send('verify_transaction_and_witness_proof', [{"block_hash": "0x02530b25ad0ff677acc365cb73de3e8cc09c7ddd58272e879252e199d08df83b", "transactions_proof": {"indices": ["0x0"], "lemmas": ["0x4a3e8d3f1c4b9a2e5d8b7c4f9a3e6b1d5c8a2f4e7b1d6c9a3e5b8d2f4a6c8e0b"]}, "witnesses_proof": {"indices": ["0x0"], "lemmas": ["0x8a7e3d3f1c4b9a2e5d8b7c4f9a3e6b1d5c8a2f4e7b1d6c9a3e5b8d2f4a6c8e0b"]}}]);
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
    'method': 'verify_transaction_and_witness_proof',
    'params': [{"block_hash": "0x02530b25ad0ff677acc365cb73de3e8cc09c7ddd58272e879252e199d08df83b", "transactions_proof": {"indices": ["0x0"], "lemmas": ["0x4a3e8d3f1c4b9a2e5d8b7c4f9a3e6b1d5c8a2f4e7b1d6c9a3e5b8d2f4a6c8e0b"]}, "witnesses_proof": {"indices": ["0x0"], "lemmas": ["0x8a7e3d3f1c4b9a2e5d8b7c4f9a3e6b1d5c8a2f4e7b1d6c9a3e5b8d2f4a6c8e0b"]}}]
}})
print(response.json())
```
{% endtab %}
{% endtabs %}
