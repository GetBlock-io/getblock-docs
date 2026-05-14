---
description: >-
  Example code for the getblocktransactionsext JSON-RPC method. Сomplete guide
  on how to use getblocktransactionsext JSON-RPC in GetBlock.io Web3
  documentation.
---

# getblocktransactionsext - TON

This method returns the list of transactions in a block, with extended information for each transaction, avoiding the need for a second round trip to fetch full transaction details.

## Parameters

| Parameter   | Type    | Required | Description                                                             |
| ----------- | ------- | -------- | ----------------------------------------------------------------------- |
| workchain   | integer | Yes      | Workchain ID                                                            |
| shard       | string  | Yes      | Shard ID as a string-encoded int64                                      |
| seqno       | integer | Yes      | Block sequence number                                                   |
| root\_hash  | string  | No       | Expected root hash of the block                                         |
| file\_hash  | string  | No       | Expected file hash of the block                                         |
| after\_lt   | string  | No       | Logical time of the transaction to read after                           |
| after\_hash | string  | No       | Account hash within the block, indicating the transaction to read after |
| count       | integer | No       | Maximum number of items to return (default: 40)                         |

## Request Example

{% tabs %}
{% tab title="cURL" %}
**REST (GET):**

```bash
curl --location --request GET 'https://go.getblock.io/<ACCESS-TOKEN>/getBlockTransactionsExt?workchain=-1&shard=-9223372036854775808&seqno=45792554' \
--header 'Content-Type: application/json'
```

**JSON-RPC (POST):**

```bash
curl --location --request POST 'https://go.getblock.io/<ACCESS-TOKEN>' \
--header 'Content-Type: application/json' \
--data-raw '{
    "jsonrpc": "2.0",
    "method": "getBlockTransactionsExt",
    "params": {
        "workchain": -1,
        "shard": "-9223372036854775808",
        "seqno": 45792554
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
    "method": "getBlockTransactionsExt",
    "params": {
        "workchain": -1,
        "shard": "-9223372036854775808",
        "seqno": 45792554
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
    "method": "getBlockTransactionsExt",
    "params": {
        "workchain": -1,
        "shard": "-9223372036854775808",
        "seqno": 45792554
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
        "method": "getBlockTransactionsExt",
        "params": {
                "workchain": -1,
                "shard": "-9223372036854775808",
                "seqno": 45792554
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
        "@type": "blocks.transactionsExt",
        "id": {
            "@type": "ton.blockIdExt",
            "workchain": -1,
            "shard": "-9223372036854775808",
            "seqno": 45792554
        },
        "req_count": 40,
        "incomplete": false,
        "transactions": [
            {
                "@type": "raw.transaction",
                "utime": 1737869732,
                "transaction_id": {
                    "lt": "47652103000003",
                    "hash": "AbCdEf\u2026"
                },
                "fee": "5000000",
                "in_msg": {
                    "source": "EQ\u2026",
                    "destination": "EQDtFpEwcFAEcRe5mLVh2N6C0x-_hJEM7W61_JLnSF74p4q2",
                    "value": "1000000000"
                },
                "out_msgs": []
            }
        ]
    },
    "id": "getblock.io",
    "jsonrpc": "2.0"
}
```
{% endcode %}

## Response Parameters

| Field                            | Type    | Description                                         |
| -------------------------------- | ------- | --------------------------------------------------- |
| result.transactions              | array   | Array of full transaction objects                   |
| result.transactions\[].utime     | integer | Unix timestamp at which the transaction was applied |
| result.transactions\[].fee       | string  | Total fee paid (nanotons)                           |
| result.transactions\[].in\_msg   | object  | Incoming message (if any)                           |
| result.transactions\[].out\_msgs | array   | Outgoing messages                                   |

## Use Cases

* Block-by-block indexing where full transaction detail is needed
* Avoiding the N+1 round-trips of `getBlockTransactions` + per-tx fetch
* High-throughput indexers and analytics

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
