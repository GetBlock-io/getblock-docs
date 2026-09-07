---
tags:
  - kusama
---

# chain\_getHeader - Kusama

Returns the header of a block by hash, or the latest header when the hash is omitted, including parent hash, number, state root, extrinsics root, and digest logs.

{% hint style="info" %}
Available over both the JSON-RPC (HTTP) and WebSocket interfaces.
{% endhint %}

## Parameters

| Parameter | Type   | Required | Description                            |
| --------- | ------ | -------- | -------------------------------------- |
| hash      | string | Optional | Block hash; omit for the latest header |

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
    "method": "chain_getHeader",
    "params": [
    "0x8a1e6d2f0b7c4a9e3d5f1b8c2a6e4d0f9b3c7a5e1d8f2b6c4a0e9d3f7b1c5a2e6"
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
    method: 'chain_getHeader',
    params: [
    "0x8a1e6d2f0b7c4a9e3d5f1b8c2a6e4d0f9b3c7a5e1d8f2b6c4a0e9d3f7b1c5a2e6"
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
        'method': 'chain_getHeader',
        'params': [
    "0x8a1e6d2f0b7c4a9e3d5f1b8c2a6e4d0f9b3c7a5e1d8f2b6c4a0e9d3f7b1c5a2e6"
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
            "method": "chain_getHeader",
            "params": [
    "0x8a1e6d2f0b7c4a9e3d5f1b8c2a6e4d0f9b3c7a5e1d8f2b6c4a0e9d3f7b1c5a2e6"
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
        "parentHash": "0x...",
        "number": "0x17f7b40",
        "stateRoot": "0x...",
        "extrinsicsRoot": "0x...",
        "digest": {
            "logs": [
                "0x..."
            ]
        }
    }
}
```

## Response Fields

| Field      | Type   | Description              |
| ---------- | ------ | ------------------------ |
| number     | string | Block number (hex)       |
| parentHash | string | Hash of the parent block |
| stateRoot  | string | State trie root          |

## Use Cases

* **Chain Following**: Read the latest header height
* **Light Reads**: Fetch header without full extrinsics
* **Verification**: Check parent linkage and roots

## Error Handling

| Error                     | Message       | Description                                       |
| ------------------------- | ------------- | ------------------------------------------------- |
| -32603 / Internal error   | Unknown block | No block matches the supplied hash                |
| 403 / RBAC: access denied | Access denied | The GetBlock access token is missing or incorrect |
