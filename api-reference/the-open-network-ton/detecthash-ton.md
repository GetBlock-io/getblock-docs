---
description: >-
  Example code for the detecthash JSON-RPC method. Сomplete guide on how to use
  detecthash JSON-RPC in GetBlock.io Web3 documentation.
---

# detecthash - TON

This method returns all possible representations of a 256-bit hash: hex, base64, and URL-safe base64.

## Parameters

| Parameter | Type   | Required | Description                                                |
| --------- | ------ | -------- | ---------------------------------------------------------- |
| hash      | string | Yes      | 256-bit hash in any form (hex, base64, or URL-safe base64) |

## Request Example

{% tabs %}
{% tab title="cURL" %}
**REST (GET):**

```bash
curl --location --request GET 'https://go.getblock.io/<ACCESS-TOKEN>/detectHash?hash=0E5F84CDDFCC0A3B23F18EDC6E70C45F40DDFCB6C8E97D43F8A1023AA873F2D7' \
--header 'Content-Type: application/json'
```

**JSON-RPC (POST):**

```bash
curl --location --request POST 'https://go.getblock.io/<ACCESS-TOKEN>/jsonRPC' \
--header 'Content-Type: application/json' \
--data-raw '{
    "jsonrpc": "2.0",
    "method": "detectHash",
    "params": {
        "hash": "0E5F84CDDFCC0A3B23F18EDC6E70C45F40DDFCB6C8E97D43F8A1023AA873F2D7"
    },
    "id": "getblock.io"
}'
```
{% endtab %}

{% tab title="JavaScript (Axios)" %}
```javascript
import axios from 'axios';

const data = JSON.stringify({
    "jsonrpc": "2.0",
    "method": "detectHash",
    "params": {
        "hash": "0E5F84CDDFCC0A3B23F18EDC6E70C45F40DDFCB6C8E97D43F8A1023AA873F2D7"
    },
    "id": "getblock.io"
});

const config = {
    method: 'post',
    url: 'https://go.getblock.io/<ACCESS-TOKEN>/jsonRPC',
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

url = "https://go.getblock.io/<ACCESS-TOKEN>/jsonRPC"

payload = json.dumps({
    "jsonrpc": "2.0",
    "method": "detectHash",
    "params": {
        "hash": "0E5F84CDDFCC0A3B23F18EDC6E70C45F40DDFCB6C8E97D43F8A1023AA873F2D7"
    },
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
        "method": "detectHash",
        "params": {
                "hash": "0E5F84CDDFCC0A3B23F18EDC6E70C45F40DDFCB6C8E97D43F8A1023AA873F2D7"
        },
        "id": "getblock.io"
});

    let response = client
        .post("https://go.getblock.io/<ACCESS-TOKEN>/jsonRPC")
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
    "ok": true,
    "result": {
        "hex": "0E5F84CDDFCC0A3B23F18EDC6E70C45F40DDFCB6C8E97D43F8A1023AA873F2D7",
        "b64": "Dl+EzfzMCjsj8Y7cbnDEX0Dt/LbI6X1D+KECOqhz8tc=",
        "b64url": "Dl-Ezfz_CjsjY7cbnDEX0Dt_LbI6X1D-KECOqhz8tc="
    },
    "id": "getblock.io",
    "jsonrpc": "2.0"
}
```
{% endcode %}

## Response Parameters

| Field         | Type   | Description                    |
| ------------- | ------ | ------------------------------ |
| result.hex    | string | Hex representation of the hash |
| result.b64    | string | Standard base64 representation |
| result.b64url | string | URL-safe base64 representation |

## Use Cases

* Normalising hashes received from different sources
* Converting between hex and base64 for SDK compatibility

## Error Handling

| Status Code | Error Message       | Cause                                       |
| ----------- | ------------------- | ------------------------------------------- |
| 403         | Forbidden           | Missing or invalid `<ACCESS-TOKEN>`         |
| 422         | Validation Error    | Request parameters are missing or malformed |
| 429         | Too Many Requests   | Rate limit exceeded for your plan           |
| 504         | Lite Server Timeout | Upstream TON liteserver timed out           |

## SDK Integration

{% tabs %}
{% tab title="@ton/ton (JavaScript)" %}
```javascript
import { TonClient } from '@ton/ton';

const client = new TonClient({
    endpoint: 'https://go.getblock.io/<ACCESS-TOKEN>/jsonRPC'
});

// The @ton/ton client wraps the same JSON-RPC methods.
// For low-level access, use client.callGetMethod or client.provider.
```
{% endtab %}

{% tab title="pytoniq (Python)" %}
```python
from pytoniq import LiteClient
# For HTTP API access, use ton-http-client or call the JSON-RPC
# endpoint directly with requests as shown in the Request Example.
```
{% endtab %}
{% endtabs %}
