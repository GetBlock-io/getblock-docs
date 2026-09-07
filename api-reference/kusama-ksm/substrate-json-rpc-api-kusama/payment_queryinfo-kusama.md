---
tags:
  - kusama
---

# payment\_queryInfo - Kusama

Returns the fee information for a signed extrinsic without submitting it: its dispatch weight, class, and the estimated partial fee. Use it to show a fee estimate before broadcasting.

{% hint style="info" %}
Available over both the JSON-RPC (HTTP) and WebSocket interfaces.
{% endhint %}

## Parameters

| Parameter | Type   | Required | Description                     |
| --------- | ------ | -------- | ------------------------------- |
| extrinsic | string | Yes      | Hex-encoded signed extrinsic    |
| at        | string | Optional | Block hash; omit for the latest |

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
    "method": "payment_queryInfo",
    "params": [
    "0x4d028400d43593c715fdd31c61141abd04a99fd6822c8558854ccde39a5684e7a56da27d..."
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
    method: 'payment_queryInfo',
    params: [
    "0x4d028400d43593c715fdd31c61141abd04a99fd6822c8558854ccde39a5684e7a56da27d..."
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
        'method': 'payment_queryInfo',
        'params': [
    "0x4d028400d43593c715fdd31c61141abd04a99fd6822c8558854ccde39a5684e7a56da27d..."
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
            "method": "payment_queryInfo",
            "params": [
    "0x4d028400d43593c715fdd31c61141abd04a99fd6822c8558854ccde39a5684e7a56da27d..."
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
        "weight": {
            "refTime": 195000000,
            "proofSize": 3593
        },
        "class": "normal",
        "partialFee": "1600000000"
    }
}
```

## Response Fields

| Field      | Type   | Description                                     |
| ---------- | ------ | ----------------------------------------------- |
| weight     | object | Dispatch weight (refTime and proofSize)         |
| class      | string | Dispatch class (normal, operational, mandatory) |
| partialFee | string | Estimated fee in Planck (10^-12 KSM)            |

## Use Cases

* **Fee Preview**: Show an estimated fee before signing
* **Budgeting**: Size a user's balance against the fee
* **Weight Analysis**: Read dispatch weight for a call

## Error Handling

| Error                     | Message       | Description                                       |
| ------------------------- | ------------- | ------------------------------------------------- |
| -32603 / Internal error   | Bad extrinsic | The extrinsic could not be decoded                |
| 403 / RBAC: access denied | Access denied | The GetBlock access token is missing or incorrect |
