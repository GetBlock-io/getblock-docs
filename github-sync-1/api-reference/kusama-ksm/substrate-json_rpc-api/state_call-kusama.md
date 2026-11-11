---
description: >-
  Example code for the state_call JSON-RPC method. Complete guide on how to use
  state_call JSON-RPC in GetBlock Web3 documentation.
---

# state\_call - Kusama

Calls a runtime API entry point with SCALE-encoded arguments at a block and returns the SCALE-encoded result. Underlies higher-level queries such as fee estimation and account info.

{% hint style="info" %}
Available over both the JSON-RPC (HTTP) and WebSocket interfaces.
{% endhint %}

## Parameters

| Parameter | Type   | Required | Description                                              |
| --------- | ------ | -------- | -------------------------------------------------------- |
| name      | string | Yes      | Runtime API method, e.g. AccountNonceApi\_account\_nonce |
| bytes     | string | Yes      | Hex-encoded SCALE arguments                              |
| at        | string | Optional | Block hash; omit for latest                              |

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
    "method": "state_call",
    "params": [
    "AccountNonceApi_account_nonce",
    "0x0000000000000000000000000000000000000000000000000000000000000000"
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
    method: 'state_call',
    params: [
    "AccountNonceApi_account_nonce",
    "0x0000000000000000000000000000000000000000000000000000000000000000"
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
        'method': 'state_call',
        'params': [
    "AccountNonceApi_account_nonce",
    "0x0000000000000000000000000000000000000000000000000000000000000000"
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
            "method": "state_call",
            "params": [
    "AccountNonceApi_account_nonce",
    "0x0000000000000000000000000000000000000000000000000000000000000000"
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
    "result": "0x2a000000"
}
```

## Response Fields

| Field  | Type   | Description                            |
| ------ | ------ | -------------------------------------- |
| result | string | SCALE-encoded runtime API result (hex) |

## Use Cases

* **Runtime APIs**: Call any exposed runtime API directly
* **Custom Queries**: Reach data not exposed by a dedicated RPC
* **Tooling**: Build typed helpers over runtime APIs

## Error Handling

| Error                     | Message         | Description                                       |
| ------------------------- | --------------- | ------------------------------------------------- |
| -32603 / Internal error   | Execution error | The runtime API name or arguments are invalid     |
| 403 / RBAC: access denied | Access denied   | The GetBlock access token is missing or incorrect |
