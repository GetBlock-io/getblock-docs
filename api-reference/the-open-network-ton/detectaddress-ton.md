---
description: >-
  Example code for the detectaddress JSON-RPC method. Сomplete guide on how to
  use detectaddress JSON-RPC in GetBlock.io Web3 documentation.
---

# detectaddress - TON

This method returns all possible representations of a TON address: raw (`workchain:hex`), user-friendly bounceable (`EQ…`), user-friendly non-bounceable (`UQ…`), URL-safe encodings, and testnet variants.

## Parameters

| Parameter | Type   | Required | Description         |
| --------- | ------ | -------- | ------------------- |
| address   | string | Yes      | Address in any form |

## Request Example

{% tabs %}
{% tab title="cURL" %}
**REST (GET):**

{% code overflow="wrap" %}
```bash
curl --location --request GET 'https://go.getblock.io/<ACCESS-TOKEN>/detectAddress?address=EQDtFpEwcFAEcRe5mLVh2N6C0x-_hJEM7W61_JLnSF74p4q2' \
--header 'Content-Type: application/json'
```
{% endcode %}

**JSON-RPC (POST):**

```bash
curl --location --request POST 'https://go.getblock.io/<ACCESS-TOKEN>/jsonRPC' \
--header 'Content-Type: application/json' \
--data-raw '{
    "jsonrpc": "2.0",
    "method": "detectAddress",
    "params": {
        "address": "EQDtFpEwcFAEcRe5mLVh2N6C0x-_hJEM7W61_JLnSF74p4q2"
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
    "method": "detectAddress",
    "params": {
        "address": "EQDtFpEwcFAEcRe5mLVh2N6C0x-_hJEM7W61_JLnSF74p4q2"
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
    "method": "detectAddress",
    "params": {
        "address": "EQDtFpEwcFAEcRe5mLVh2N6C0x-_hJEM7W61_JLnSF74p4q2"
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
        "method": "detectAddress",
        "params": {
                "address": "EQDtFpEwcFAEcRe5mLVh2N6C0x-_hJEM7W61_JLnSF74p4q2"
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
        "raw_form": "0:ed16913070500471175e992b561d8de82d31fbf84910ced6eb5fc92e74485ef8a",
        "bounceable": {
            "b64": "EQDtFpEwcFAEcRe5mLVh2N6C0x+/hJEM7W61/JLnSF74p4q2",
            "b64url": "EQDtFpEwcFAEcRe5mLVh2N6C0x-_hJEM7W61_JLnSF74p4q2"
        },
        "non_bounceable": {
            "b64": "UQDtFpEwcFAEcRe5mLVh2N6C0x+/hJEM7W61/JLnSF74p2nm",
            "b64url": "UQDtFpEwcFAEcRe5mLVh2N6C0x-_hJEM7W61_JLnSF74p2nm"
        },
        "given_type": "friendly_bounceable",
        "test_only": false
    },
    "id": "getblock.io",
    "jsonrpc": "2.0"
}
```
{% endcode %}

## Response Parameters

| Field                         | Type    | Description                                             |
| ----------------------------- | ------- | ------------------------------------------------------- |
| result.raw\_form              | string  | Raw `workchain:hex` representation                      |
| result.bounceable.b64         | string  | User-friendly bounceable address (`EQ…`) in base64      |
| result.bounceable.b64url      | string  | User-friendly bounceable address in URL-safe base64     |
| result.non\_bounceable.b64    | string  | User-friendly non-bounceable address (`UQ…`) in base64  |
| result.non\_bounceable.b64url | string  | User-friendly non-bounceable address in URL-safe base64 |
| result.given\_type            | string  | Type of the address that was supplied                   |
| result.test\_only             | boolean | Whether the address belongs to the testnet              |

## Use Cases

* Normalising user-entered addresses across forms
* Detecting whether a user pasted a testnet address
* Switching between bounceable and non-bounceable forms based on context (e.g. depositing to a new wallet)

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
