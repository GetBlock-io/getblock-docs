---
description: >-
  Example code for the admin_validatorKey JSON-RPC method. Complete guide on how
  to use admin_validatorKey JSON-RPC in GetBlock Web3 documentation.
---

# admin\_validatorKey - Tempo

Returns the node's ed25519 validator public key if configured, or `null` for non-validator nodes. Available only on self-hosted Tempo nodes started with `--http.api admin`.

{% hint style="warning" %}
**SELF-HOSTED-ONLY METHOD.** This method is part of the `admin_*` namespace and is only available on self-hosted Tempo nodes started with `--http.api admin`. It is **not** available on any shared or managed RPC provider, including GetBlock. Documented here for completeness of the Tempo RPC surface; for operational reasons it cannot be exposed on multi-tenant infrastructure.
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
    "method": "admin_validatorKey",
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
    "method": "admin_validatorKey",
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
    "method": "admin_validatorKey",
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
        "method": "admin_validatorKey",
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
    "result": "0x8e1aa0e2d4c7f9d4b0c8b8d5f1a2b3c4d5e6f7a8b9c0d1e2f3a4b5c6d7e8f9a0"
}
```
{% endcode %}

## Response Parameters

| Field  | Type           | Description                                                                          |
| ------ | -------------- | ------------------------------------------------------------------------------------ |
| result | string or null | Hex-encoded ed25519 validator public key, or `null` if not configured as a validator |

## Use Cases

* Validator operations — confirming the correct keypair is loaded
* Validator monitoring scripts running locally on the validator host

## Error Handling

| Status Code | Error Message     | Cause                                                                               |
| ----------- | ----------------- | ----------------------------------------------------------------------------------- |
| 403         | Forbidden         | Missing or invalid `<ACCESS-TOKEN>`                                                 |
| -32602      | Invalid params    | Request parameters are missing or malformed                                         |
| 429         | Too Many Requests | Rate limit exceeded for your plan                                                   |
| -32601      | Method not found  | The `admin` API is not enabled on this node — not exposed on managed infrastructure |
