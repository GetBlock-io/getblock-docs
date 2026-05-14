---
description: >-
  Example code for the sendbocreturnhash JSON-RPC method. Сomplete guide on how
  to use sendbocreturnhash JSON-RPC in GetBlock.io Web3 documentation.
---

# sendbocreturnhash - TON

This method broadcasts a serialized BOC to the TON blockchain and returns the hash of the external message, allowing the caller to track its inclusion via `tryLocateTx` or `getTransactions`.

## Parameters

| Parameter | Type   | Required | Description                                       |
| --------- | ------ | -------- | ------------------------------------------------- |
| boc       | string | Yes      | Base64-encoded BOC of the signed external message |

## Request Example

{% tabs %}
{% tab title="cURL" %}
**JSON-RPC (POST):**

{% code overflow="wrap" %}
```bash
curl --location --request POST 'https://go.getblock.io/<ACCESS-TOKEN>' \
--header 'Content-Type: application/json' \
--data-raw '{
    "jsonrpc": "2.0",
    "method": "sendBocReturnHash",
    "params": {
        "boc": "te6cckEBAQEAUgAAn/8AIN0gggFMl7qXMO1E0NcLH+Ck8mCDCNcYINMf0x/THwL4I7vyZO1E0NMf0x/T//QE0VFDuvKhUVG68qIG+QFUEHb5EPKj+CO78mPtRNDXCx/jAFETuvKh1AAQc8L/8tCD"
    },
    "id": "getblock.io"
}'
```
{% endcode %}
{% endtab %}

{% tab title="JavaScript (Axios)" %}
```javascript
import axios from 'axios';

const data = JSON.stringify({
    "jsonrpc": "2.0",
    "method": "sendBocReturnHash",
    "params": {
        "boc": "te6cckEBAQEAUgAAn/8AIN0gggFMl7qXMO1E0NcLH+Ck8mCDCNcYINMf0x/THwL4I7vyZO1E0NMf0x/T//QE0VFDuvKhUVG68qIG+QFUEHb5EPKj+CO78mPtRNDXCx/jAFETuvKh1AAQc8L/8tCD"
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
    "method": "sendBocReturnHash",
    "params": {
        "boc": "te6cckEBAQEAUgAAn/8AIN0gggFMl7qXMO1E0NcLH+Ck8mCDCNcYINMf0x/THwL4I7vyZO1E0NMf0x/T//QE0VFDuvKhUVG68qIG+QFUEHb5EPKj+CO78mPtRNDXCx/jAFETuvKh1AAQc8L/8tCD"
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
        "method": "sendBocReturnHash",
        "params": {
                "boc": "te6cckEBAQEAUgAAn/8AIN0gggFMl7qXMO1E0NcLH+Ck8mCDCNcYINMf0x/THwL4I7vyZO1E0NMf0x/T//QE0VFDuvKhUVG68qIG+QFUEHb5EPKj+CO78mPtRNDXCx/jAFETuvKh1AAQc8L/8tCD"
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
        "@type": "raw.extMessageInfo",
        "hash": "AbCdEfGhIjKlMnOpQrStUvWxYz0123456789+/="
    },
    "id": "getblock.io",
    "jsonrpc": "2.0"
}
```
{% endcode %}

## Response Parameters

| Field       | Type   | Description                                                        |
| ----------- | ------ | ------------------------------------------------------------------ |
| result.hash | string | Hash of the broadcast external message (base64), used for tracking |

## Use Cases

* Broadcasting and tracking transfers in one step
* Wallet UIs that display a pending tx hash immediately
* Server-side automation that polls for inclusion

## Error Handling

| Status Code | Error Message               | Cause                                       |
| ----------- | --------------------------- | ------------------------------------------- |
| 403         | Forbidden                   | Missing or invalid `<ACCESS-TOKEN>`         |
| 422         | Validation Error            | Request parameters are missing or malformed |
| 429         | Too Many Requests           | Rate limit exceeded for your plan           |
| 504         | Lite Server Timeout         | Upstream TON liteserver timed out           |
| 422         | Validation Error            | BOC payload is malformed or invalid base64  |
| 500         | Liteserver rejected message | Message failed validity checks              |

## SDK Integration

{% tabs %}
{% tab title="@ton/ton (JavaScript)" %}
```javascript
import { TonClient } from '@ton/ton';

const client = new TonClient({
    endpoint: 'https://go.getblock.io/<ACCESS-TOKEN>'
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
