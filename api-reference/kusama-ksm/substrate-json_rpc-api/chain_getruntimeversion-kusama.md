---
description: >-
  Example code for the chain_getRuntimeVersion JSON-RPC method. Complete guide
  on how to use chain_getRuntimeVersion JSON-RPC in GetBlock Web3 documentation.
---

# chain\_getRuntimeVersion - Kusama

Returns the runtime version at a block (or latest) via the chain namespace — the same data as state\_getRuntimeVersion, provided for compatibility.

{% hint style="info" %}
Available over both the JSON-RPC (HTTP) and WebSocket interfaces.
{% endhint %}

## Parameters

| Parameter | Type   | Required | Description                     |
| --------- | ------ | -------- | ------------------------------- |
| hash      | string | Optional | Block hash; omit for the latest |

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
    "method": "chain_getRuntimeVersion",
    "params": []
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
    method: 'chain_getRuntimeVersion',
    params: []
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
        'method': 'chain_getRuntimeVersion',
        'params': []
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
            "method": "chain_getRuntimeVersion",
            "params": []
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
        "specName": "kusama",
        "implName": "parity-kusama",
        "specVersion": 1003000,
        "transactionVersion": 26,
        "apis": [
            [
                "0xdf6acb689907609b",
                5
            ]
        ]
    }
}
```

## Response Fields

| Field              | Type    | Description              |
| ------------------ | ------- | ------------------------ |
| specVersion        | integer | Runtime spec version     |
| transactionVersion | integer | Extrinsic format version |

## Use Cases

* **Signing**: Read spec/transaction version for the signed payload
* **Upgrade Detection**: Detect runtime upgrades
* **Compatibility**: Fallback for clients using the chain namespace

## Error Handling

| Error                     | Message       | Description                                       |
| ------------------------- | ------------- | ------------------------------------------------- |
| -32603 / Internal error   | Unknown block | The block hash is not known                       |
| 403 / RBAC: access denied | Access denied | The GetBlock access token is missing or incorrect |
