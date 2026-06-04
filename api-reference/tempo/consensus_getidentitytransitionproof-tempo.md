---
description: >-
  Example code for the consensus_getIdentityTransitionProof JSON-RPC method.
  Complete guide on how to use consensus_getIdentityTransitionProof JSON-RPC in
  GetBlock Web3 documentation.
---

# consensus\_getIdentityTransitionProof - Tempo

Returns DKG (Distributed Key Generation) identity transition proofs. Used by light clients and bridges to verify validator set transitions across epochs. By default returns only the most recent transition; pass `full=true` to walk all transitions back to genesis.

{% hint style="warning" %}
**VALIDATOR-NODE-ONLY METHOD.** This method is part of the `consensus_*` namespace and is only available on Tempo validator nodes. It is **not** available on shared RPC infrastructure such as GetBlock's standard shared endpoints. Calling this method against a non-validator node returns `-32601 Method not found`. If you need access, contact GetBlock for dedicated validator node access.
{% endhint %}

## Parameters

| Parameter   | Type            | Required | Description                                                                                           |
| ----------- | --------------- | -------- | ----------------------------------------------------------------------------------------------------- |
| from\_epoch | integer or null | No       | Epoch to search from (defaults to latest finalized)                                                   |
| full        | boolean or null | No       | If `true`, return all transitions back to genesis; if `false` or null (default), only the most recent |

## Request Example

{% tabs %}
{% tab title="cURL" %}
```bash
curl --location --request POST 'https://go.getblock.io/<ACCESS-TOKEN>/' \
--header 'Content-Type: application/json' \
--data-raw '{
    "jsonrpc": "2.0",
    "method": "consensus_getIdentityTransitionProof",
    "params": [
        null,
        null
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
    "method": "consensus_getIdentityTransitionProof",
    "params": [
        null,
        null
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
    "method": "consensus_getIdentityTransitionProof",
    "params": [
        null,
        null
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
        "method": "consensus_getIdentityTransitionProof",
        "params": [
                null,
                null
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
        "identity": "0x8e1aa0e2d4c7f9d4b0c8b8d5f1a2b3c4d5e6f7a8b9c0d1e2f3a4b5c6d7e8f9a0b1c2d3e4f5a6b7c8d9e0f1a2b3c4",
        "transitions": [
            {
                "transitionEpoch": 42,
                "oldIdentity": "0x7d4c2e0f1a3b5c7d9e1f2a4b6c8d0e2f3a4b5c6d7e8f9a0b1c2d3e4f5a6b7c8d9e0f1a2b3c4d5e6f7a8b9c0d1e2f3",
                "newIdentity": "0x8e1aa0e2d4c7f9d4b0c8b8d5f1a2b3c4d5e6f7a8b9c0d1e2f3a4b5c6d7e8f9a0b1c2d3e4f5a6b7c8d9e0f1a2b3c4",
                "proof": {
                    "header": {
                        "number": "0x9c40",
                        "hash": "0x6baa8fa86b9a7e5d3f1c8a4b6e9d2f5a8c3e7b1d4f9a6c2e5b8d3f7a1c4e9b2d"
                    },
                    "certificate": "0xc4b5a6d7e8f9a0b1c2d3e4f5a6b7c8d9e0f1a2b3c4d5e6f7a8b9c0d1e2f3a4b5c6"
                }
            }
        ]
    }
}
```
{% endcode %}

## Response Parameters

| Field                                 | Type                        | Description                                                            |
| ------------------------------------- | --------------------------- | ---------------------------------------------------------------------- |
| result.identity                       | string                      | Hex-encoded BLS public key at the requested epoch                      |
| result.transitions                    | array of IdentityTransition | Transitions ordered newest to oldest                                   |
| result.transitions\[].transitionEpoch | integer                     | Epoch where the DKG ceremony occurred                                  |
| result.transitions\[].oldIdentity     | string                      | BLS public key before the transition                                   |
| result.transitions\[].newIdentity     | string                      | BLS public key after the transition                                    |
| result.transitions\[].proof           | object                      | Block header + finalization certificate. Omitted for genesis (epoch 0) |

## Use Cases

* Light client validator-set verification across epochs
* Cross-chain bridges proving Tempo validator transitions
* Audit tools verifying DKG ceremony integrity

## Error Handling

| Status Code | Error Message     | Cause                                                        |
| ----------- | ----------------- | ------------------------------------------------------------ |
| 403         | Forbidden         | Missing or invalid `<ACCESS-TOKEN>`                          |
| -32602      | Invalid params    | Request parameters are missing or malformed                  |
| -32601      | Method not found  | Method does not exist or is not enabled on this node         |
| 429         | Too Many Requests | Rate limit exceeded for your plan                            |
| -32601      | Method not found  | Validator-only method — not exposed on shared infrastructure |

## SDK Integration

{% tabs %}
{% tab title="JavaScript (Axios)" %}
```javascript
import axios from 'axios';

const response = await axios.post('https://go.getblock.io/<ACCESS-TOKEN>/', {
    jsonrpc: '2.0',
    id: 'getblock.io',
    method: 'consensus_getIdentityTransitionProof',
    params: [null, null]
}});
console.log(response.data);
```
{% endtab %}

{% tab title="Python (Requests)" %}
```python
import requests

response = requests.post('https://go.getblock.io/<ACCESS-TOKEN>/', json={
    'jsonrpc': '2.0',
    'id': 'getblock.io',
    'method': 'consensus_getIdentityTransitionProof',
    'params': [null, null]
}})
print(response.json())
```
{% endtab %}
{% endtabs %}
