---
description: >-
  Example code for the getblockheader JSON-RPC method. Сomplete guide on how to
  use getblockheader JSON-RPC in GetBlock.io Web3 documentation.
---

# getblockheader - TON

This method returns the header of a specific block (workchain, shard, seqno). Optionally, `root_hash` and `file_hash` can be provided to verify the block identity.

## Parameters

| Parameter  | Type    | Required | Description                                        |
| ---------- | ------- | -------- | -------------------------------------------------- |
| workchain  | integer | Yes      | Workchain ID                                       |
| shard      | string  | Yes      | Shard ID as a string-encoded int64                 |
| seqno      | integer | Yes      | Block sequence number                              |
| root\_hash | string  | No       | Expected root hash of the block (for verification) |
| file\_hash | string  | No       | Expected file hash of the block (for verification) |

## Request Example

{% tabs %}
{% tab title="cURL" %}
**REST (GET):**

```bash
curl --location --request GET 'https://go.getblock.io/<ACCESS-TOKEN>/getBlockHeader?workchain=-1&shard=-9223372036854775808&seqno=45792554' \
--header 'Content-Type: application/json'
```

**JSON-RPC (POST):**

```bash
curl --location --request POST 'https://go.getblock.io/<ACCESS-TOKEN>/' \
--header 'Content-Type: application/json' \
--data-raw '{
    "jsonrpc": "2.0",
    "method": "getBlockHeader",
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
    "method": "getBlockHeader",
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
    "method": "getBlockHeader",
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
        "method": "getBlockHeader",
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
        "@type": "blocks.header",
        "id": {
            "@type": "ton.blockIdExt",
            "workchain": -1,
            "shard": "-9223372036854775808",
            "seqno": 45792554,
            "root_hash": "\u2026",
            "file_hash": "\u2026"
        },
        "global_id": -239,
        "version": 0,
        "after_merge": false,
        "after_split": false,
        "before_split": false,
        "want_merge": true,
        "want_split": false,
        "validator_list_hash_short": 123456789,
        "catchain_seqno": 1234,
        "min_ref_mc_seqno": 45792500,
        "is_key_block": false,
        "prev_key_block_seqno": 45790000,
        "start_lt": "47652103000000",
        "end_lt": "47652103000099",
        "gen_utime": 1737869732,
        "prev_blocks": [
            {
                "@type": "ton.blockIdExt",
                "workchain": -1,
                "shard": "-9223372036854775808",
                "seqno": 45792553,
                "root_hash": "\u2026",
                "file_hash": "\u2026"
            }
        ]
    },
    "id": "getblock.io",
    "jsonrpc": "2.0"
}
```
{% endcode %}

## Response Parameters

| Field                 | Type    | Description                                                  |
| --------------------- | ------- | ------------------------------------------------------------ |
| result.id             | object  | Block ID this header describes                               |
| result.global\_id     | integer | Network global ID (`-239` for TON mainnet, `-3` for testnet) |
| result.gen\_utime     | integer | Block generation Unix timestamp                              |
| result.start\_lt      | string  | Starting logical time within the block                       |
| result.end\_lt        | string  | Ending logical time within the block                         |
| result.is\_key\_block | boolean | Whether the block is a key block (carries config updates)    |
| result.prev\_blocks   | array   | Block IDs immediately preceding this block                   |

## Use Cases

* Verifying a block's identity and parent chain
* Detecting key blocks (config changes)
* Building light clients and SPV proofs
* Indexing block-level metadata

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
