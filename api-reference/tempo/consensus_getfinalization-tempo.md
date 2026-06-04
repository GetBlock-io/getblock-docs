---
description: >-
  Example code for the consensus_getFinalization JSON-RPC method. Complete guide
  on how to use consensus_getFinalization JSON-RPC in GetBlock Web3
  documentation.
---

# consensus\_getFinalization - Tempo

Gets a finalized block by height or `"latest"`. Returns a `CertifiedBlock` object including the BLS finalization certificate, consensus epoch and view, and the full Tempo block. Used by bridges, indexers, and explorers that need verifiable finality proofs.

{% hint style="warning" %}
**VALIDATOR-NODE-ONLY METHOD.** This method is part of the `consensus_*` namespace and is only available on Tempo validator nodes. It is **not** available on shared RPC infrastructure such as GetBlock's standard shared endpoints. Calling this method against a non-validator node returns `-32601 Method not found`. If you need access, contact GetBlock for dedicated validator node access.
{% endhint %}

## Parameters

| Parameter | Type             | Required | Description                                                            |
| --------- | ---------------- | -------- | ---------------------------------------------------------------------- |
| query     | string or object | Yes      | Either the literal string `"latest"` or an object `{"height": number}` |

## Request Example

{% tabs %}
{% tab title="cURL" %}
```bash
curl --location --request POST 'https://go.getblock.io/<ACCESS-TOKEN>/' \
--header 'Content-Type: application/json' \
--data-raw '{
    "jsonrpc": "2.0",
    "method": "consensus_getFinalization",
    "params": [
        "latest"
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
    "method": "consensus_getFinalization",
    "params": [
        "latest"
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
    "method": "consensus_getFinalization",
    "params": [
        "latest"
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
        "method": "consensus_getFinalization",
        "params": [
                "latest"
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
        "epoch": 42,
        "view": 387213,
        "digest": "0x6baa8fa86b9a7e5d3f1c8a4b6e9d2f5a8c3e7b1d4f9a6c2e5b8d3f7a1c4e9b2d",
        "certificate": "0xc4b5a6d7e8f9a0b1c2d3e4f5a6b7c8d9e0f1a2b3c4d5e6f7a8b9c0d1e2f3a4b5c6",
        "block": {
            "number": "0x9c40",
            "hash": "0x6baa8fa86b9a7e5d3f1c8a4b6e9d2f5a8c3e7b1d4f9a6c2e5b8d3f7a1c4e9b2d",
            "timestamp": "0x67891f24",
            "transactions": []
        }
    }
}
```
{% endcode %}

## Response Parameters

| Field              | Type                   | Description                                                             |
| ------------------ | ---------------------- | ----------------------------------------------------------------------- |
| result             | CertifiedBlock \| null | Certified block object, or `null` if no matching finalization exists    |
| result.epoch       | integer                | Consensus epoch in which the block was finalized                        |
| result.view        | integer                | Consensus view in which the block was finalized                         |
| result.digest      | string                 | Block digest (32 bytes hex)                                             |
| result.certificate | string                 | Hex-encoded BLS finalization certificate — verifiable proof of finality |
| result.block       | object                 | The full Tempo block (same shape as `eth_getBlockByNumber`)             |

## Use Cases

* Cross-chain bridges proving Tempo finality on other chains
* Light clients verifying that a block is irreversibly final
* Indexers that need consensus-layer finality proofs in addition to block data

## Error Handling

| Status Code | Error Message     | Cause                                                                  |
| ----------- | ----------------- | ---------------------------------------------------------------------- |
| 403         | Forbidden         | Missing or invalid `<ACCESS-TOKEN>`                                    |
| -32602      | Invalid params    | Request parameters are missing or malformed                            |
| -32601      | Method not found  | Method does not exist or is not enabled on this node                   |
| 429         | Too Many Requests | Rate limit exceeded for your plan                                      |
| -32601      | Method not found  | This is a validator-only method — not exposed on shared infrastructure |
