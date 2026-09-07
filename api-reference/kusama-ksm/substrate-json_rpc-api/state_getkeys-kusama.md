---
description: >-
  Example code for the state_getKeys JSON-RPC method. Complete guide on how to
  use state_getKeys JSON-RPC in GetBlock Web3 documentation.
---

# state\_getKeys - Kusama

Returns all storage keys that start with a prefix at a block. For large maps, use `state_getKeysPaged` instead to avoid oversized responses.

{% hint style="info" %}
Available over both the JSON-RPC (HTTP) and WebSocket interfaces.
{% endhint %}

## Parameters

| Parameter | Type   | Required | Description                 |
| --------- | ------ | -------- | --------------------------- |
| prefix    | string | Yes      | Hex key prefix              |
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
    "method": "state_getKeys",
    "params": [
    "0x26aa394eea5630e07c48ae0c9558cef7"
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
    method: 'state_getKeys',
    params: [
    "0x26aa394eea5630e07c48ae0c9558cef7"
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
        'method': 'state_getKeys',
        'params': [
    "0x26aa394eea5630e07c48ae0c9558cef7"
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
            "method": "state_getKeys",
            "params": [
    "0x26aa394eea5630e07c48ae0c9558cef7"
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
    "result": [
        "0x26aa394eea5630e07c48ae0c9558cef7b99d880ec681799c0cf30e8886371da9..."
    ]
}
```

## Response Fields

| Field  | Type  | Description                          |
| ------ | ----- | ------------------------------------ |
| result | array | Hex storage keys matching the prefix |

## Use Cases

* **Key Discovery**: Find storage keys under a pallet prefix
* **Small Maps**: Enumerate a small storage map
* **Debugging**: Inspect which keys exist

## Error Handling

| Error                     | Message       | Description                                       |
| ------------------------- | ------------- | ------------------------------------------------- |
| -32603 / Internal error   | Unknown block | The block hash is not known                       |
| 403 / RBAC: access denied | Access denied | The GetBlock access token is missing or incorrect |
