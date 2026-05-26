---
description: >-
  Example code for the get_last_block_header JSON-RPC method. Complete guide on
  how to use get_last_block_header JSON-RPC in GetBlock Web3 documentation.
---

# get\_last\_block\_header - Monero

This method returns the header of the most recent block in the chain. Useful as a lightweight alternative to `get_info` when only block-level metadata is needed.

## Parameters

* None

## Request Example

{% tabs %}
{% tab title="cURL" %}
```bash
curl --location --request POST 'https://go.getblock.io/<ACCESS-TOKEN>/' \
--header 'Content-Type: application/json' \
--data-raw '{
    "jsonrpc": "2.0",
    "method": "get_last_block_header",
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
    "method": "get_last_block_header",
    "params": {},
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
    "method": "get_last_block_header",
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
        "method": "get_last_block_header",
        "params": {},
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
        "block_header": {
            "block_size": 5500,
            "depth": 0,
            "difficulty": 294558564173,
            "hash": "e22cf75f39ae720e8b71b3d120a5ac03f0db50bba6379e2850975b4859190bc6",
            "height": 3194582,
            "major_version": 16,
            "minor_version": 16,
            "nonce": 1234567890,
            "num_txes": 12,
            "orphan_status": false,
            "prev_hash": "37155cae7d3aa8ca7dc8b9e69e0d1f64b5e3a8b1c2d9e4f5a6b7c8d9e0f1a2b3",
            "reward": 600000000000,
            "timestamp": 1737869732
        },
        "status": "OK",
        "untrusted": false
    }
}
```
{% endcode %}

## Response Parameters

| Field                               | Type         | Description                                      |
| ----------------------------------- | ------------ | ------------------------------------------------ |
| result.block\_header.hash           | string       | Hash of the latest block                         |
| result.block\_header.height         | unsigned int | Height of the latest block                       |
| result.block\_header.timestamp      | unsigned int | Unix timestamp when the block was mined          |
| result.block\_header.difficulty     | unsigned int | Mining difficulty at this block                  |
| result.block\_header.reward         | unsigned int | Coinbase reward in atomic units                  |
| result.block\_header.num\_txes      | unsigned int | Number of non-coinbase transactions in the block |
| result.block\_header.major\_version | unsigned int | Major protocol version                           |

## Use Cases

* Lightweight chain-tip polling
* Displaying network statistics in dashboards
* Computing block-time analytics

## Error Handling

| Status Code | Error Message     | Cause                               |
| ----------- | ----------------- | ----------------------------------- |
| 404         | Not found         | Missing or invalid `<ACCESS-TOKEN>` |
| 429         | Too Many Requests | Rate limit exceeded for your plan   |

## SDK Integration

{% tabs %}
{% tab title="monero-javascript" %}
```javascript
import monerojs from 'monero-javascript';

const daemon = await monerojs.connectToDaemonRpc('https://go.getblock.io/<ACCESS-TOKEN>/');

// Most methods are exposed directly on the daemon client. For raw access:
const result = await daemon.getDaemonConnection().sendJsonRequest('get_last_block_header', {});
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
result = backend.raw_request('get_last_block_header', {})
print(result)
```
{% endtab %}
{% endtabs %}
