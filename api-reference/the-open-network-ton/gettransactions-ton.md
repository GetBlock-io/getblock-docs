---
description: >-
  Example code for the gettransactions JSON-RPC method. Сomplete guide on how to
  use gettransactions JSON-RPC in GetBlock.io Web3 documentation.
---

# gettransactions - TON

This method returns historical transactions for a given address. Pagination uses the `lt` (logical time) and `hash` cursor, with `to_lt` as an inclusive lower bound. Set `archival=true` to query historical data older than the standard retention window.

## Parameters

| Parameter | Type    | Required | Description                                            |
| --------- | ------- | -------- | ------------------------------------------------------ |
| address   | string  | Yes      | Address to query                                       |
| limit     | integer | No       | Maximum number of transactions to return (default: 10) |
| lt        | string  | No       | Logical time of the transaction to start from          |
| hash      | string  | No       | Transaction hash to start from                         |
| to\_lt    | string  | No       | Logical time to stop at (default: 0)                   |
| archival  | boolean | No       | Whether to use an archival node (default: false)       |

## Request Example

{% tabs %}
{% tab title="cURL" %}
**REST (GET):**

```bash
curl --location --request GET 'https://go.getblock.io/<ACCESS-TOKEN>/getTransactions?address=EQDtFpEwcFAEcRe5mLVh2N6C0x-_hJEM7W61_JLnSF74p4q2&limit=10' \
--header 'Content-Type: application/json'
```

**JSON-RPC (POST):**

```bash
curl --location --request POST 'https://go.getblock.io/<ACCESS-TOKEN>/' \
--header 'Content-Type: application/json' \
--data-raw '{
    "jsonrpc": "2.0",
    "method": "getTransactions",
    "params": {
        "address": "EQDtFpEwcFAEcRe5mLVh2N6C0x-_hJEM7W61_JLnSF74p4q2",
        "limit": 10
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
    "method": "getTransactions",
    "params": {
        "address": "EQDtFpEwcFAEcRe5mLVh2N6C0x-_hJEM7W61_JLnSF74p4q2",
        "limit": 10
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
    "method": "getTransactions",
    "params": {
        "address": "EQDtFpEwcFAEcRe5mLVh2N6C0x-_hJEM7W61_JLnSF74p4q2",
        "limit": 10
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
        "method": "getTransactions",
        "params": {
                "address": "EQDtFpEwcFAEcRe5mLVh2N6C0x-_hJEM7W61_JLnSF74p4q2",
                "limit": 10
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
    "result": [
        {
            "@type": "raw.transaction",
            "utime": 1737869732,
            "data": "te6cckECEgEAAlMAART\u2026",
            "transaction_id": {
                "@type": "internal.transactionId",
                "lt": "47652103000003",
                "hash": "AbCdEf\u2026"
            },
            "fee": "5000000",
            "storage_fee": "1000000",
            "other_fee": "4000000",
            "in_msg": {
                "@type": "raw.message",
                "source": "EQ\u2026",
                "destination": "EQDtFpEwcFAEcRe5mLVh2N6C0x-_hJEM7W61_JLnSF74p4q2",
                "value": "1000000000",
                "fwd_fee": "100000",
                "ihr_fee": "0",
                "created_lt": "47652103000000",
                "body_hash": "\u2026",
                "msg_data": {
                    "@type": "msg.dataRaw",
                    "body": "te6cckEBAQEA\u2026"
                },
                "message": ""
            },
            "out_msgs": []
        }
    ],
    "id": "getblock.io",
    "jsonrpc": "2.0"
}
```
{% endcode %}

## Response Parameters

| Field                          | Type    | Description                                             |
| ------------------------------ | ------- | ------------------------------------------------------- |
| result                         | array   | Array of transactions, newest first                     |
| result\[].utime                | integer | Unix timestamp at which the transaction was applied     |
| result\[].transaction\_id.lt   | string  | Logical time of the transaction                         |
| result\[].transaction\_id.hash | string  | Hash of the transaction (base64)                        |
| result\[].fee                  | string  | Total fee paid (nanotons)                               |
| result\[].in\_msg              | object  | Incoming message that triggered the transaction, if any |
| result\[].out\_msgs            | array   | Outgoing messages sent by the transaction               |

## Use Cases

* Wallet transaction history views
* Indexers and explorers per-address feeds
* Bookkeeping and accounting systems
* Detecting incoming Jetton or NFT transfers

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
