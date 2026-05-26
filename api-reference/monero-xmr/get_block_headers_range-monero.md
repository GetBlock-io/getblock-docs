---
description: >-
  Example code for the get_block_headers_range JSON-RPC method. Complete guide
  on how to use get_block_headers_range JSON-RPC in GetBlock Web3 documentation.
---

# get\_block\_headers\_range - Monero

This method returns block headers for all blocks in a given range, inclusive at both ends. Use it to efficiently fetch headers in bulk for indexing.

## Parameters

| Parameter     | Type         | Required | Description                                 |
| ------------- | ------------ | -------- | ------------------------------------------- |
| start\_height | unsigned int | Yes      | First block height in the range (inclusive) |
| end\_height   | unsigned int | Yes      | Last block height in the range (inclusive)  |

## Request Example

{% tabs %}
{% tab title="cURL" %}
```bash
curl --location --request POST 'https://go.getblock.io/<ACCESS-TOKEN>/' \
--header 'Content-Type: application/json' \
--data-raw '{
    "jsonrpc": "2.0",
    "method": "get_block_headers_range",
    "params": {
        "start_height": 1545999,
        "end_height": 1546000
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
    "method": "get_block_headers_range",
    "params": {
        "start_height": 1545999,
        "end_height": 1546000
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
    "method": "get_block_headers_range",
    "params": {
        "start_height": 1545999,
        "end_height": 1546000
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
        "method": "get_block_headers_range",
        "params": {
                "start_height": 1545999,
                "end_height": 1546000
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
    "id": "getblock.io",
    "jsonrpc": "2.0",
    "result": {
        "headers": [
            {
                "block_size": 5500,
                "depth": 1300,
                "difficulty": 200000000000,
                "hash": "aaa\u2026",
                "height": 1545999,
                "major_version": 7,
                "minor_version": 7,
                "nonce": 12345,
                "num_txes": 0,
                "orphan_status": false,
                "prev_hash": "bbb\u2026",
                "reward": 1300000000000,
                "timestamp": 1547069360
            },
            {
                "block_size": 5500,
                "depth": 1299,
                "difficulty": 200000000000,
                "hash": "ccc\u2026",
                "height": 1546000,
                "major_version": 7,
                "minor_version": 7,
                "nonce": 67890,
                "num_txes": 2,
                "orphan_status": false,
                "prev_hash": "aaa\u2026",
                "reward": 1300000000000,
                "timestamp": 1547069480
            }
        ],
        "status": "OK",
        "untrusted": false
    }
}
```
{% endcode %}

## Response Parameters

| Field                       | Type         | Description                                   |
| --------------------------- | ------------ | --------------------------------------------- |
| result.headers              | array        | Array of block headers in the requested range |
| result.headers\[].height    | unsigned int | Height of this block                          |
| result.headers\[].hash      | string       | Block hash                                    |
| result.headers\[].timestamp | unsigned int | Unix timestamp                                |
| result.headers\[].reward    | unsigned int | Coinbase reward in atomic units               |

## Use Cases

* Bulk-indexing block metadata efficiently
* Backfilling block analytics dashboards
* Computing rolling block-time statistics

## Error Handling

| Status Code | Error Message     | Cause                                                         |
| ----------- | ----------------- | ------------------------------------------------------------- |
| 404         | Not found         | Missing or invalid `<ACCESS-TOKEN>`                           |
| -32602      | Invalid params    | Request parameters are missing or malformed                   |
| 429         | Too Many Requests | Rate limit exceeded for your plan                             |
| -2          | Invalid range     | `end_height` is less than `start_height` or exceeds chain tip |

## SDK Integration

{% tabs %}
{% tab title="monero-javascript" %}
```javascript
import monerojs from 'monero-javascript';

const daemon = await monerojs.connectToDaemonRpc('https://go.getblock.io/<ACCESS-TOKEN>/');

// Most methods are exposed directly on the daemon client. For raw access:
const result = await daemon.getDaemonConnection().sendJsonRequest('get_block_headers_range', {"start_height": 1545999, "end_height": 1546000});
console.log(result);
```
{% endtab %}

{% tab title="monero-python" %}
```python
from monero.backends.jsonrpc import JSONRPCDaemon
from monero.daemon import Daemon

backend = JSONRPCDaemon(host='go.getblock.io', port=443, path='/<ACCESS-TOKEN>/', protocol='https')
daemon = Daemon(backend)

# For methods covered by the typed API, use the daemon attribute directly.
# For raw RPC access:
result = backend.raw_request('get_block_headers_range', {"start_height": 1545999, "end_height": 1546000})
print(result)
```
{% endtab %}
{% endtabs %}
