---
description: >-
  Example code for the state_getStorageHash JSON-RPC method. Complete guide on
  how to use state_getStorageHash JSON-RPC in GetBlock Web3 documentation.
---

# state\_getStorageHash - Kusama

Returns the Blake2 hash of the value stored at a key, at a block. Useful to cheaply detect whether a value changed without transferring the value itself.

{% hint style="info" %}
Available over both the JSON-RPC (HTTP) and WebSocket interfaces.
{% endhint %}

## Parameters

| Parameter | Type   | Required | Description                 |
| --------- | ------ | -------- | --------------------------- |
| key       | string | Yes      | Hex storage key             |
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
    "method": "state_getStorageHash",
    "params": [
    "0x26aa394eea5630e07c48ae0c9558cef7b99d880ec681799c0cf30e8886371da9"
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
    method: 'state_getStorageHash',
    params: [
    "0x26aa394eea5630e07c48ae0c9558cef7b99d880ec681799c0cf30e8886371da9"
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
        'method': 'state_getStorageHash',
        'params': [
    "0x26aa394eea5630e07c48ae0c9558cef7b99d880ec681799c0cf30e8886371da9"
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
            "method": "state_getStorageHash",
            "params": [
    "0x26aa394eea5630e07c48ae0c9558cef7b99d880ec681799c0cf30e8886371da9"
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
    "result": "0x9f2b4c1d8e7a6f5b3c2d1e0f9a8b7c6d5e4f3a2b1c0d9e8f7a6b5c4d3e2f1a0b"
}
```

## Response Fields

| Field  | Type   | Description |
| ------ | ------ | ----------- |
| result | string | null        |

## Use Cases

* **Change Detection**: Compare hashes to detect changes cheaply
* **Caching**: Invalidate caches on hash change
* **Bandwidth**: Avoid fetching large values

## Error Handling

| Error                     | Message       | Description                                       |
| ------------------------- | ------------- | ------------------------------------------------- |
| -32603 / Internal error   | Unknown block | The block hash is not known                       |
| 403 / RBAC: access denied | Access denied | The GetBlock access token is missing or incorrect |
