---
description: >-
  Example code for the state_getReadProof JSON-RPC method. Complete guide on how
  to use state_getReadProof JSON-RPC in GetBlock Web3 documentation.
---

# state\_getReadProof - Kusama

Returns a Merkle (trie) read proof for a set of storage keys at a block. Light clients use it to verify state without trusting the node.

{% hint style="info" %}
Available over both the JSON-RPC (HTTP) and WebSocket interfaces.
{% endhint %}

## Parameters

| Parameter | Type   | Required | Description                 |
| --------- | ------ | -------- | --------------------------- |
| keys      | array  | Yes      | Array of hex storage keys   |
| at        | string | Optional | Block hash; omit for latest |

## Request

{% tabs %}
{% tab title="cURL" %}
{% code overflow="wrap" %}
```bash
curl --location --request POST 'https://shared.eu-central-1.getblock.io/<ACCESS-TOKEN>/' \
--header 'Content-Type: application/json' \
--data-raw '{
    "jsonrpc": "2.0",
    "id": "getblock.io",
    "method": "state_getReadProof",
    "params": [
    [
        "0x26aa394eea5630e07c48ae0c9558cef7b99d880ec681799c0cf30e8886371da9"
    ]
]
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
    id: 'getblock.io',
    method: 'state_getReadProof',
    params: [
    [
        "0x26aa394eea5630e07c48ae0c9558cef7b99d880ec681799c0cf30e8886371da9"
    ]
]
}, { headers: { 'Content-Type': 'application/json' } });

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
        'id': 'getblock.io',
        'method': 'state_getReadProof',
        'params': [
    [
        "0x26aa394eea5630e07c48ae0c9558cef7b99d880ec681799c0cf30e8886371da9"
    ]
]
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
        .json(&json!({
            "jsonrpc": "2.0",
            "id": "getblock.io",
            "method": "state_getReadProof",
            "params": [
    [
        "0x26aa394eea5630e07c48ae0c9558cef7b99d880ec681799c0cf30e8886371da9"
    ]
]
        }))
        .send().await?
        .json::<Value>().await?;
    println!("{}", response["result"]);
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
        "at": "0x8a1e6d2f0b7c4a9e3d5f1b8c2a6e4d0f9b3c7a5e1d8f2b6c4a0e9d3f7b1c5a2e6",
        "proof": [
            "0x3f0e...",
            "0x8a1c..."
        ]
    }
}
```

## Response Fields

| Field | Type   | Description                                       |
| ----- | ------ | ------------------------------------------------- |
| at    | string | Block hash the proof is anchored to               |
| proof | array  | Array of hex-encoded trie nodes forming the proof |

## Use Cases

* **Light Clients**: Verify storage without full trust in the node
* **Bridges**: Prove state to another chain
* **Audits**: Cryptographically confirm a value

## Error Handling

| Error                     | Message       | Description                                       |
| ------------------------- | ------------- | ------------------------------------------------- |
| -32603 / Internal error   | Unknown block | The block hash is not known                       |
| 403 / RBAC: access denied | Access denied | The GetBlock access token is missing or incorrect |
