---
description: >-
  Example code for the estimatefee JSON-RPC method. Сomplete guide on how to use
  estimatefee JSON-RPC in GetBlock.io Web3 documentation.
---

# estimatefee - TON

This method estimates the fees that a given message would incur if applied to the specified contract, without actually broadcasting it. Use it to give users an accurate fee preview before signing.

## Parameters

| Parameter      | Type    | Required | Description                                                            |
| -------------- | ------- | -------- | ---------------------------------------------------------------------- |
| address        | string  | Yes      | Destination contract address                                           |
| body           | string  | Yes      | Base64-encoded BOC of the message body                                 |
| init\_code     | string  | No       | Base64-encoded BOC of the init code (for uninitialised contracts)      |
| init\_data     | string  | No       | Base64-encoded BOC of the init data (for uninitialised contracts)      |
| ignore\_chksig | boolean | No       | Whether to ignore the signature check during emulation (default: true) |

## Request Example

{% tabs %}
{% tab title="cURL" %}
**JSON-RPC (POST):**

```bash
curl --location --request POST 'https://go.getblock.io/<ACCESS_TOKEN>/estimateFee' \
--header 'Content-Type: application/json' \
--data-raw '{
        "address": "EQDtFpEwcFAEcRe5mLVh2N6C0x-_hJEM7W61_JLnSF74p4q2",
        "body": "te6cckEBAQEAAgAAAEysuc0=",
        "ignore_chksig": true
}'
```
{% endtab %}

{% tab title="JavaScript (Axios)" %}
```javascript
import axios from 'axios';

const data = JSON.stringify({
        "address": "EQDtFpEwcFAEcRe5mLVh2N6C0x-_hJEM7W61_JLnSF74p4q2",
        "body": "te6cckEBAQEAAgAAAEysuc0=",
        "ignore_chksig": true
});

const config = {
    method: 'post',
    url: 'https://go.getblock.io/<ACCESS-TOKEN>/estimateFee',
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

url = "https://go.getblock.io/<ACCESS-TOKEN>/estimateFee"

payload = json.dumps({
        "address": "EQDtFpEwcFAEcRe5mLVh2N6C0x-_hJEM7W61_JLnSF74p4q2",
        "body": "te6cckEBAQEAAgAAAEysuc0=",
        "ignore_chksig": true
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
                "address": "EQDtFpEwcFAEcRe5mLVh2N6C0x-_hJEM7W61_JLnSF74p4q2",
                "body": "te6cckEBAQEAAgAAAEysuc0=",
                "ignore_chksig": true
});

    let response = client
        .post("https://go.getblock.io/<ACCESS-TOKEN>/estimateFee")
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
        "@type": "query.fees",
        "source_fees": {
            "@type": "fees",
            "in_fwd_fee": 66667,
            "storage_fee": 1272,
            "gas_fee": 0,
            "fwd_fee": 0
        },
        "destination_fees": [],
        "@extra": "1778769555.4294026:0:0.3188598314708605"
    }
}
```
{% endcode %}

## Response Parameters

| Field                            | Type    | Description                          |
| -------------------------------- | ------- | ------------------------------------ |
| result.source\_fees.in\_fwd\_fee | integer | Inbound forward fee (nanotons)       |
| result.source\_fees.storage\_fee | integer | Storage fee (nanotons)               |
| result.source\_fees.gas\_fee     | integer | Gas fee (nanotons)                   |
| result.source\_fees.fwd\_fee     | integer | Outbound forward fee (nanotons)      |
| result.destination\_fees         | array   | Fees at destination accounts, if any |

## Use Cases

* Showing a user an accurate fee preview before they sign
* Server-side fee budgeting in dApps
* Validating that a contract call will not run out of gas

## Error Handling

| Status Code | Error Message       | Cause                                        |
| ----------- | ------------------- | -------------------------------------------- |
| 403         | Forbidden           | Missing or invalid `<ACCESS-TOKEN>`          |
| 422         | Validation Error    | Request parameters are missing or malformed  |
| 429         | Too Many Requests   | Rate limit exceeded for your plan            |
| 504         | Lite Server Timeout | Upstream TON liteserver timed out            |
| 422         | Validation Error    | Body, init\_code, or init\_data is malformed |

## SDK Integration

{% tabs %}
{% tab title="@ton/ton (JavaScript)" %}
```javascript
import { TonClient } from '@ton/ton';

const client = new TonClient({
    endpoint: 'https://go.getblock.io/<ACCESS-TOKEN>/estimateFee'
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
