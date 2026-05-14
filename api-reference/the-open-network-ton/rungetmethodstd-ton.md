---
description: >-
  Example code for the rungetmethodstd JSON-RPC method. Сomplete guide on how to
  use rungetmethodstd JSON-RPC in GetBlock.io Web3 documentation.
---

# rungetmethodstd - TON

This method is a standardized version of `runGetMethod` that returns a normalised TVM stack shape. Use it when you want a stable schema for parsing returned values.

## Parameters

| Parameter | Type              | Required | Description                          |
| --------- | ----------------- | -------- | ------------------------------------ |
| address   | string            | Yes      | Address of the smart contract        |
| method    | string or integer | Yes      | Method name or numeric method-id     |
| stack     | array             | Yes      | Input arguments as TVM stack entries |

## Request Example

{% tabs %}
{% tab title="cURL" %}
**JSON-RPC (POST):**

```bash
curl --location --request POST 'https://go.getblock.io/<ACCESS-TOKEN>' \
--header 'Content-Type: application/json' \
--data-raw '{
    "jsonrpc": "2.0",
    "method": "runGetMethodStd",
    "params": {
        "address": "EQDtFpEwcFAEcRe5mLVh2N6C0x-_hJEM7W61_JLnSF74p4q2",
        "method": "get_wallet_data",
        "stack": []
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
    "method": "runGetMethodStd",
    "params": {
        "address": "EQDtFpEwcFAEcRe5mLVh2N6C0x-_hJEM7W61_JLnSF74p4q2",
        "method": "get_wallet_data",
        "stack": []
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
    "method": "runGetMethodStd",
    "params": {
        "address": "EQDtFpEwcFAEcRe5mLVh2N6C0x-_hJEM7W61_JLnSF74p4q2",
        "method": "get_wallet_data",
        "stack": []
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
        "method": "runGetMethodStd",
        "params": {
                "address": "EQDtFpEwcFAEcRe5mLVh2N6C0x-_hJEM7W61_JLnSF74p4q2",
                "method": "get_wallet_data",
                "stack": []
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
        "gas_used": 1500,
        "stack": [
            {
                "type": "num",
                "value": "0x174876E800"
            }
        ],
        "exit_code": 0
    },
    "id": "getblock.io",
    "jsonrpc": "2.0"
}
```
{% endcode %}

## Response Parameters

| Field             | Type    | Description                              |
| ----------------- | ------- | ---------------------------------------- |
| result.gas\_used  | integer | Gas units consumed                       |
| result.stack      | array   | Stack entries as `{type, value}` objects |
| result.exit\_code | integer | TVM exit code                            |

## Use Cases

* Schema-stable readers for get-method results
* Tooling that prefers object-shaped stack entries over tuple-shaped

## Error Handling

| Status Code | Error Message       | Cause                                       |
| ----------- | ------------------- | ------------------------------------------- |
| 403         | Forbidden           | Missing or invalid `<ACCESS-TOKEN>`         |
| 422         | Validation Error    | Request parameters are missing or malformed |
| 429         | Too Many Requests   | Rate limit exceeded for your plan           |
| 504         | Lite Server Timeout | Upstream TON liteserver timed out           |
| 422         | Validation Error    | Method or stack types are malformed         |
| 500         | TVM execution error | Get-method returned a non-zero exit code    |

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
