---
description: >-
  Example code for the getaddressinformation JSON-RPC method. Сomplete guide on
  how to use getaddressinformation JSON-RPC in GetBlock.io Web3 documentation.
---

# getaddressinformation - TON

This method returns basic information about a TON address, including its balance in nanotons, account state, contract code, contract data, and the identifier of the last transaction. It is the most commonly used method for reading account state on TON.

## Parameters

| Parameter | Type    | Required | Description                                                                                           |
| --------- | ------- | -------- | ----------------------------------------------------------------------------------------------------- |
| address   | string  | Yes      | Identifier of the target TON account in any form (user-friendly `EQ…` / `UQ…` or raw `workchain:hex`) |
| seqno     | integer | No       | Masterchain seqno at which to read state. Defaults to the latest block                                |

## Request Example

{% tabs %}
{% tab title="cURL" %}
**REST (GET):**

```bash
curl --location --request GET 'https://go.getblock.io/<ACCESS-TOKEN>/getAddressInformation?address=EQDtFpEwcFAEcRe5mLVh2N6C0x-_hJEM7W61_JLnSF74p4q2' \
--header 'Content-Type: application/json'
```

**JSON-RPC (POST):**

```bash
curl --location --request POST 'https://go.getblock.io/<ACCESS-TOKEN>/' \
--header 'Content-Type: application/json' \
--data-raw '{
    "jsonrpc": "2.0",
    "method": "getAddressInformation",
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
    "method": "getAddressInformation",
    "params": {
        "address": "EQDtFpEwcFAEcRe5mLVh2N6C0x-_hJEM7W61_JLnSF74p4q2"
    },
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
    "method": "getAddressInformation",
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
        "method": "getAddressInformation",
        "params": {
                "address": "EQDtFpEwcFAEcRe5mLVh2N6C0x-_hJEM7W61_JLnSF74p4q2"
        },
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
    "ok": true,
    "result": {
        "@type": "raw.fullAccountState",
        "balance": "1234567890",
        "code": "te6cckEBAQEA\u2026",
        "data": "te6cckEBAQEA\u2026",
        "last_transaction_id": {
            "@type": "internal.transactionId",
            "lt": "47652103000003",
            "hash": "AbCdEfGhIjKlMnOpQrStUvWxYz0123456789+/="
        },
        "block_id": {
            "@type": "ton.blockIdExt",
            "workchain": -1,
            "shard": "-9223372036854775808",
            "seqno": 45792554
        },
        "frozen_hash": "",
        "sync_utime": 1737869732,
        "state": "active"
    },
    "id": "getblock.io",
    "jsonrpc": "2.0"
}
```
{% endcode %}

## Response Parameters

| Field                             | Type    | Description                                           |
| --------------------------------- | ------- | ----------------------------------------------------- |
| ok                                | boolean | Whether the request succeeded                         |
| result.balance                    | string  | Account balance in nanotons (1 TON = 10⁹ nanotons)    |
| result.code                       | string  | Account contract code as base64-encoded BOC           |
| result.data                       | string  | Account contract data as base64-encoded BOC           |
| result.last\_transaction\_id.lt   | string  | Logical time of the last transaction                  |
| result.last\_transaction\_id.hash | string  | Hash of the last transaction (base64)                 |
| result.state                      | string  | Account state: `active`, `uninitialized`, or `frozen` |
| result.sync\_utime                | integer | Unix timestamp at which state was synchronised        |
| id                                | string  | Request identifier echoed back                        |
| jsonrpc                           | string  | JSON-RPC protocol version (`2.0`)                     |

## Use Cases

* Wallet balance and state displays
* Smart contract state inspection
* Determining whether an account is initialised before sending a transaction
* Indexers and explorers reading account-level data
* Detecting frozen or uninitialised accounts before interaction

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
