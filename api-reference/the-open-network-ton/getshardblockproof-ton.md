---
description: >-
  Example code for the getshardblockproof JSON-RPC method. Сomplete guide on how
  to use getshardblockproof JSON-RPC in GetBlock.io Web3 documentation.
---

# getshardblockproof - TON

This method returns the Merkle proof for a shardchain block, anchored to the masterchain. Use it to verify a shard block independently of trusting the RPC provider.

## Parameters

| Parameter   | Type    | Required | Description                                                                |
| ----------- | ------- | -------- | -------------------------------------------------------------------------- |
| workchain   | integer | Yes      | Workchain ID of the shard block (`0` for basechain)                        |
| shard       | string  | Yes      | Shard ID as a 64-bit integer (string-encoded)                              |
| seqno       | integer | Yes      | Seqno of the shard block                                                   |
| from\_seqno | integer | No       | Masterchain seqno from which the proof is required. Defaults to the latest |

## Request Example

{% tabs %}
{% tab title="cURL" %}
**REST (GET):**

```bash
curl --location --request GET 'https://go.getblock.io/<ACCESS-TOKEN>/getShardBlockProof?workchain=0&shard=8000000000000000&seqno=12345678' \
--header 'Content-Type: application/json'
```

**JSON-RPC (POST):**

```bash
curl --location --request POST 'https://go.getblock.io/<ACCESS-TOKEN>/' \
--header 'Content-Type: application/json' \
--data-raw '{
    "jsonrpc": "2.0",
    "method": "getShardBlockProof",
    "params": {
        "workchain": 0,
        "shard": "8000000000000000",
        "seqno": 12345678
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
    "method": "getShardBlockProof",
    "params": {
        "workchain": 0,
        "shard": "8000000000000000",
        "seqno": 12345678
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
    "method": "getShardBlockProof",
    "params": {
        "workchain": 0,
        "shard": "8000000000000000",
        "seqno": 12345678
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
        "method": "getShardBlockProof",
        "params": {
                "workchain": 0,
                "shard": "8000000000000000",
                "seqno": 12345678
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
        "@type": "blocks.shardBlockProof",
        "from": {
            "@type": "ton.blockIdExt",
            "workchain": -1,
            "shard": "-9223372036854775808",
            "seqno": 45792554
        },
        "mc_id": {
            "@type": "ton.blockIdExt",
            "workchain": -1,
            "shard": "-9223372036854775808",
            "seqno": 45792554
        },
        "links": [
            {
                "@type": "blocks.shardBlockLink",
                "id": {
                    "workchain": 0,
                    "shard": "8000000000000000",
                    "seqno": 12345678
                },
                "proof": "te6cckECEgEAAlMAART\u2026"
            }
        ]
    },
    "id": "getblock.io",
    "jsonrpc": "2.0"
}
```
{% endcode %}

## Response Parameters

| Field         | Type   | Description                                                            |
| ------------- | ------ | ---------------------------------------------------------------------- |
| result.from   | object | Masterchain block from which the proof starts                          |
| result.mc\_id | object | Masterchain block that anchors the shard block                         |
| result.links  | array  | Array of Merkle proof links from masterchain to the target shard block |

## Use Cases

* Light client verification of shard blocks
* Building trust-minimised cross-shard bridges
* Independent verification of provider-returned shard data

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
