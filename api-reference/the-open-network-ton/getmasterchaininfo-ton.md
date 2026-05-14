---
description: >-
  Example code for the getmasterchaininfo JSON-RPC method. Сomplete guide on how
  to use getmasterchaininfo JSON-RPC in GetBlock.io Web3 documentation.
---

# getmasterchaininfo - TON

This method returns the up-to-date state of the TON masterchain, including the latest masterchain block ID. It is the TON equivalent of asking for the current chain tip and is the recommended starting point for indexers and explorers.

## Parameters

* None

## Request Example

{% tabs %}
{% tab title="cURL" %}
**REST (GET):**

```bash
curl --location --request GET 'https://go.getblock.io/<ACCESS-TOKEN>/getMasterchainInfo' \
--header 'Content-Type: application/json'
```

**JSON-RPC (POST):**

```bash
curl --location --request POST 'https://go.getblock.io/<ACCESS-TOKEN>' \
--header 'Content-Type: application/json' \
--data-raw '{
    "jsonrpc": "2.0",
    "method": "getMasterchainInfo",
    "params": {},
    "id": "getblock.io"
}'
```
{% endtab %}

{% tab title="JavaScript (Axios)" %}
```javascript
import axios from 'axios';

const data = JSON.stringify({
    "jsonrpc": "2.0",
    "method": "getMasterchainInfo",
    "params": {},
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
    "method": "getMasterchainInfo",
    "params": {},
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
        "method": "getMasterchainInfo",
        "params": {},
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
        "@type": "blocks.masterchainInfo",
        "last": {
            "@type": "ton.blockIdExt",
            "workchain": -1,
            "shard": "-9223372036854775808",
            "seqno": 45792554,
            "root_hash": "ZJiKf3xVnVz2BcGrfTuMI8tCu1iZBnGE1mNRabsHtCY=",
            "file_hash": "lZIz2njAyfQ5wW0kp9pAAEpd5cQR/QZxgZ6bRZpHnnA="
        },
        "state_root_hash": "X8Yj/jPzkH5lAYHchUTaURywL/eQrqQfx9YfHr/Tdys=",
        "init": {
            "@type": "ton.blockIdExt",
            "workchain": -1,
            "shard": "0",
            "seqno": 0,
            "root_hash": "F6OpKZKqvqeFp6CQmFomXNMfMj2EnaUSOXN+Mh+wVWk=",
            "file_hash": "XplPz01CXAps5qeSWUtxcyBfdAo5zVb1N979KLSKD24="
        }
    },
    "id": "getblock.io",
    "jsonrpc": "2.0"
}
```
{% endcode %}

## Response Parameters

| Field                    | Type    | Description                                               |
| ------------------------ | ------- | --------------------------------------------------------- |
| result.last.workchain    | integer | Workchain ID (`-1` for masterchain)                       |
| result.last.shard        | string  | Shard ID (`-9223372036854775808` is the full shard)       |
| result.last.seqno        | integer | Sequence number of the latest masterchain block           |
| result.last.root\_hash   | string  | Root hash of the latest block (base64)                    |
| result.last.file\_hash   | string  | File hash of the latest block (base64)                    |
| result.state\_root\_hash | string  | Root hash of the masterchain state                        |
| result.init              | object  | Block ID of the network's init block (genesis-equivalent) |

## Use Cases

* Determining the current chain tip for indexers
* Polling for new masterchain blocks
* Establishing a consistent read snapshot via `seqno` for subsequent calls
* Health-checking the RPC endpoint

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
