---
description: >-
  Example code for the get_block JSON-RPC method. Complete guide on how to use
  get_block JSON-RPC in GetBlock Web3 documentation.
---

# get\_block - Monero

This method returns full block information, including the block header and the list of transaction hashes. Lookup can be by height OR by hash — supply one of `height` or `hash`, not both.

## Parameters

| Parameter | Type         | Required | Description                                             |
| --------- | ------------ | -------- | ------------------------------------------------------- |
| height    | unsigned int | No\*     | The block's height (\*supply either `height` or `hash`) |
| hash      | string       | No\*     | The block's hash (\*supply either `height` or `hash`)   |

## Request Example

{% tabs %}
{% tab title="cURL" %}
```bash
curl --location --request POST 'https://go.getblock.io/<ACCESS-TOKEN>/' \
--header 'Content-Type: application/json' \
--data-raw '{
    "jsonrpc": "2.0",
    "method": "get_block",
    "params": {
        "height": 912345
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
    "method": "get_block",
    "params": {
        "height": 912345
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
    "method": "get_block",
    "params": {
        "height": 912345
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
        "method": "get_block",
        "params": {
                "height": 912345
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
        "blob": "0707e6bdfedc053771512f1bc27c62731ae9e8f2443db64ce742f4e57f5cf8d393de28551e441a0000000002fb830a\u2026",
        "block_header": {
            "block_size": 5500,
            "depth": 100,
            "difficulty": 200000000000,
            "hash": "e22cf75f39ae720e8b71b3d120a5ac03f0db50bba6379e2850975b4859190bc6",
            "height": 912345,
            "major_version": 7,
            "minor_version": 7,
            "nonce": 1234567890,
            "num_txes": 1,
            "orphan_status": false,
            "prev_hash": "37155cae7d3aa8ca7dc8b9e69e0d1f64b5e3a8b1c2d9e4f5a6b7c8d9e0f1a2b3",
            "reward": 1500000000000,
            "timestamp": 1452793716
        },
        "json": "{\n  \"major_version\": 7, \n  \"minor_version\": 7, \n  \"timestamp\": 1452793716\u2026",
        "miner_tx_hash": "e22cf75f39ae720e8b71b3d120a5ac03f0db50bba6379e2850975b4859190bc6",
        "tx_hashes": [
            "d6e48158472848e6687173a91ae6eebfa3e1d778e65252ee99d7515d63090408"
        ],
        "status": "OK",
        "untrusted": false
    }
}
```
{% endcode %}

## Response Parameters

| Field                  | Type            | Description                                                    |
| ---------------------- | --------------- | -------------------------------------------------------------- |
| result.blob            | string          | Hex blob of the entire block                                   |
| result.block\_header   | object          | Decoded block header                                           |
| result.json            | string          | Decoded block as a JSON string (note: stringified, not parsed) |
| result.miner\_tx\_hash | string          | Hash of the coinbase (miner) transaction                       |
| result.tx\_hashes      | array of string | Hashes of non-coinbase transactions in the block               |

## Use Cases

* Block-by-block indexing with full transaction listing
* Block explorer back-ends
* Replaying block contents in research and analytics

## Error Handling

| Status Code | Error Message     | Cause                                       |
| ----------- | ----------------- | ------------------------------------------- |
| 403         | Forbidden         | Missing or invalid `<ACCESS-TOKEN>`         |
| -32602      | Invalid params    | Request parameters are missing or malformed |
| 429         | Too Many Requests | Rate limit exceeded for your plan           |

## SDK Integration

{% tabs %}
{% tab title="monero-javascript" %}
```javascript
import monerojs from 'monero-javascript';

const daemon = await monerojs.connectToDaemonRpc('https://go.getblock.io/<ACCESS-TOKEN>/');

// Most methods are exposed directly on the daemon client. For raw access:
const result = await daemon.getDaemonConnection().sendJsonRequest('get_block', {"height": 912345});
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
result = backend.raw_request('get_block', {"height": 912345})
print(result)
```
{% endtab %}
{% endtabs %}
