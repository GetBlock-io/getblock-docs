---
description: >-
  Example code for the getlibraries JSON-RPC method. Сomplete guide on how to
  use getlibraries JSON-RPC in GetBlock.io Web3 documentation.
---

# getlibraries - TON

This method returns library code by their hashes. TON smart contracts can reference shared library code by hash; this method retrieves those library cells.

## Parameters

| Parameter | Type            | Required | Description                                     |
| --------- | --------------- | -------- | ----------------------------------------------- |
| libraries | array of string | No       | Array of 256-bit library hashes (base64 or hex) |

## Request Example

{% tabs %}
{% tab title="cURL" %}
**REST (GET):**

```bash
curl --location --request GET 'https://go.getblock.io/<ACCESS-TOKEN>/getLibraries?libraries=0E5F84CDDFCC0A3B23F18EDC6E70C45F40DDFCB6C8E97D43F8A1023AA873F2D7' \
--header 'Content-Type: application/json'
```

**JSON-RPC (POST):**

```bash
curl --location --request POST 'https://go.getblock.io/<ACCESS-TOKEN>' \
--header 'Content-Type: application/json' \
--data-raw '{
    "jsonrpc": "2.0",
    "method": "getLibraries",
    "params": {
        "libraries": [
            "0E5F84CDDFCC0A3B23F18EDC6E70C45F40DDFCB6C8E97D43F8A1023AA873F2D7"
        ]
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
    "method": "getLibraries",
    "params": {
        "libraries": [
            "0E5F84CDDFCC0A3B23F18EDC6E70C45F40DDFCB6C8E97D43F8A1023AA873F2D7"
        ]
    },
    "id": "getblock.io"
});

const config = {
    method: 'post',
    url: 'https://go.getblock.io/<ACCESS-TOKEN>',
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

url = "https://go.getblock.io/<ACCESS-TOKEN>"

payload = json.dumps({
    "jsonrpc": "2.0",
    "method": "getLibraries",
    "params": {
        "libraries": [
            "0E5F84CDDFCC0A3B23F18EDC6E70C45F40DDFCB6C8E97D43F8A1023AA873F2D7"
        ]
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
        "method": "getLibraries",
        "params": {
                "libraries": [
                        "0E5F84CDDFCC0A3B23F18EDC6E70C45F40DDFCB6C8E97D43F8A1023AA873F2D7"
                ]
        },
        "id": "getblock.io"
});

    let response = client
        .post("https://go.getblock.io/<ACCESS-TOKEN>")
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
        "@type": "smc.libraryResult",
        "result": [
            {
                "hash": "0E5F84CDDFCC0A3B23F18EDC6E70C45F40DDFCB6C8E97D43F8A1023AA873F2D7",
                "data": "te6cckEBAQEA\u2026"
            }
        ]
    },
    "id": "getblock.io",
    "jsonrpc": "2.0"
}
```
{% endcode %}

## Response Parameters

| Field                 | Type   | Description                            |
| --------------------- | ------ | -------------------------------------- |
| result.result         | array  | Array of library entries               |
| result.result\[].hash | string | Library hash                           |
| result.result\[].data | string | Base64-encoded BOC of the library code |

## Use Cases

* Resolving library references during off-chain contract emulation
* Building TVM emulators and debuggers
* Static analysis of contracts that use shared libraries

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
    endpoint: 'https://go.getblock.io/<ACCESS-TOKEN>/'
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
