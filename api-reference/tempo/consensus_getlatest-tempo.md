---
description: >-
  Example code for the consensus_getLatest JSON-RPC method. Complete guide on
  how to use consensus_getLatest JSON-RPC in GetBlock Web3 documentation.
---

# consensus\_getLatest - Tempo

Returns the current consensus state snapshot: the latest finalized block and (if ahead) the latest notarized block. Useful for monitoring consensus liveness and catching reorgs at the consensus layer (though Tempo's deterministic finality means notarized → finalized is fast and reliable).

{% hint style="warning" %}
**VALIDATOR-NODE-ONLY METHOD.** This method is part of the `consensus_*` namespace and is only available on Tempo validator nodes. It is **not** available on shared RPC infrastructure such as GetBlock's standard shared endpoints. Calling this method against a non-validator node returns `-32601 Method not found`. If you need access, contact GetBlock for dedicated validator node access.
{% endhint %}

## Parameters

* None

## Request Example

{% tabs %}
{% tab title="cURL" %}
```bash
curl --location --request POST 'https://go.getblock.io/<ACCESS-TOKEN>/' \
--header 'Content-Type: application/json' \
--data-raw '{
    "jsonrpc": "2.0",
    "method": "consensus_getLatest",
    "params": [],
    "id": "getblock.io"
}'
```
{% endtab %}

{% tab title="JavaScript (Axios)" %}
```javascript
import axios from 'axios';

const data = JSON.stringify({
    "jsonrpc": "2.0",
    "method": "consensus_getLatest",
    "params": [],
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
    "method": "consensus_getLatest",
    "params": [],
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
        "method": "consensus_getLatest",
        "params": [],
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
        "finalized": {
            "epoch": 42,
            "view": 387213,
            "digest": "0x6baa8fa86b9a7e5d3f1c8a4b6e9d2f5a8c3e7b1d4f9a6c2e5b8d3f7a1c4e9b2d",
            "certificate": "0xc4b5a6d7e8f9a0b1c2d3e4f5a6b7c8d9e0f1a2b3c4d5e6f7a8b9c0d1e2f3a4b5c6",
            "block": {
                "number": "0x9c40",
                "hash": "0x6baa8fa86b9a7e5d3f1c8a4b6e9d2f5a8c3e7b1d4f9a6c2e5b8d3f7a1c4e9b2d"
            }
        },
        "notarized": {
            "epoch": 42,
            "view": 387214,
            "digest": "0x4f3a1d6e8c2b9a7e5d3f1c8a4b6e9d2f5a8c3e7b1d4f9a6c2e5b8d3f7a1c4e9b",
            "certificate": "0xa1b2c3d4...",
            "block": {
                "number": "0x9c41",
                "hash": "0x4f3a1d6e8c2b9a7e..."
            }
        }
    }
}
```
{% endcode %}

## Response Parameters

| Field              | Type                     | Description                                                     |
| ------------------ | ------------------------ | --------------------------------------------------------------- |
| `result.finalized` | `CertifiedBlock \| null` | Latest finalized block                                          |
| `result.notarized` | `CertifiedBlock \| null` | Latest notarized block, if ahead of finalized; `null` otherwise |

## Use Cases

* Validator-side consensus liveness monitoring
* Detecting notarized-but-not-yet-finalized blocks for low-latency UX
* Building consensus-layer dashboards

## Error Handling

| Status Code | Error Message     | Cause                                                                  |
| ----------- | ----------------- | ---------------------------------------------------------------------- |
| 403         | Forbidden         | Missing or invalid `<ACCESS-TOKEN>`                                    |
| -32602      | Invalid params    | Request parameters are missing or malformed                            |
| -32601      | Method not found  | Method does not exist or is not enabled on this node                   |
| 429         | Too Many Requests | Rate limit exceeded for your plan                                      |
| -32601      | Method not found  | This is a validator-only method — not exposed on shared infrastructure |
