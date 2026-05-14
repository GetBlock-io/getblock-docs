---
description: >-
  Example code for the lookupblock JSON-RPC method. Сomplete guide on how to use
  lookupblock JSON-RPC in GetBlock.io Web3 documentation.
---

# lookupblock - TON

This method looks up a block by some combination of workchain, shard, seqno, logical time, and Unix timestamp. At least one of `seqno`, `lt`, or `unixtime` must be provided alongside the required `workchain` and `shard`.

## Parameters

| Parameter | Type    | Required | Description                                            |
| --------- | ------- | -------- | ------------------------------------------------------ |
| workchain | integer | Yes      | Workchain ID (`-1` for masterchain, `0` for basechain) |
| shard     | string  | Yes      | Shard ID as a 64-bit integer (string-encoded)          |
| seqno     | integer | No       | Block sequence number to look up                       |
| lt        | string  | No       | Logical time of the block (string-encoded int64)       |
| unixtime  | integer | No       | Unix timestamp of the block                            |

## Request Example

{% tabs %}
{% tab title="cURL" %}
**REST (GET):**

```bash
curl --location --request GET 'https://go.getblock.io/<ACCESS-TOKEN>/lookupBlock?workchain=-1&shard=-9223372036854775808&seqno=45792554' \
--header 'Content-Type: application/json'
```

**JSON-RPC (POST):**

```bash
curl --location --request POST 'https://go.getblock.io/<ACCESS-TOKEN>' \
--header 'Content-Type: application/json' \
--data-raw '{
    "jsonrpc": "2.0",
    "method": "lookupBlock",
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
    "method": "lookupBlock",
    "params": {
        "workchain": -1,
        "shard": "-9223372036854775808",
        "seqno": 45792554
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

url = "https://go.getblock.io/<ACCESS-TOKEN>"

payload = json.dumps({
    "jsonrpc": "2.0",
    "method": "lookupBlock",
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
        "method": "lookupBlock",
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
        "@type": "ton.blockIdExt",
        "workchain": -1,
        "shard": "-9223372036854775808",
        "seqno": 45792554,
        "root_hash": "ZJiKf3xVnVz2BcGrfTuMI8tCu1iZBnGE1mNRabsHtCY=",
        "file_hash": "lZIz2njAyfQ5wW0kp9pAAEpd5cQR/QZxgZ6bRZpHnnA="
    },
    "id": "getblock.io",
    "jsonrpc": "2.0"
}
```
{% endcode %}

## Response Parameters

| Field             | Type    | Description                              |
| ----------------- | ------- | ---------------------------------------- |
| result.workchain  | integer | Workchain ID of the resolved block       |
| result.shard      | string  | Shard ID of the resolved block           |
| result.seqno      | integer | Resolved block sequence number           |
| result.root\_hash | string  | Root hash of the resolved block (base64) |
| result.file\_hash | string  | File hash of the resolved block (base64) |

## Use Cases

* Resolving a block ID from a timestamp for historical queries
* Finding the block that contains a given logical time
* Bridging seqno-based and time-based access patterns

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
