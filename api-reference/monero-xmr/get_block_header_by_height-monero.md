# get\_block\_header\_by\_height monero

This method returns the header of a block at a given height. Functionally similar to `get_block_header_by_hash` but indexed by height.

## Parameters

| Parameter | Type         | Required | Description                                |
| --------- | ------------ | -------- | ------------------------------------------ |
| height    | unsigned int | Yes      | Height of the block whose header to return |

## Request Example

{% tabs %}
{% tab title="cURL" %}
```bash
curl --location --request POST 'https://go.getblock.io/<ACCESS-TOKEN>/' \
--header 'Content-Type: application/json' \
--data-raw '{
    "jsonrpc": "2.0",
    "method": "get_block_header_by_height",
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
    "method": "get_block_header_by_height",
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
    "method": "get_block_header_by_height",
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
        "method": "get_block_header_by_height",
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

```json
{
    "id": "getblock.io",
    "jsonrpc": "2.0",
    "result": {
        "block_header": {
            "block_size": 5500,
            "depth": 100,
            "difficulty": 294558564173,
            "hash": "e22cf75f39ae720e8b71b3d120a5ac03f0db50bba6379e2850975b4859190bc6",
            "height": 912345,
            "major_version": 7,
            "minor_version": 7,
            "nonce": 1234567890,
            "num_txes": 12,
            "orphan_status": false,
            "prev_hash": "37155cae7d3aa8ca7dc8b9e69e0d1f64b5e3a8b1c2d9e4f5a6b7c8d9e0f1a2b3",
            "reward": 1500000000000,
            "timestamp": 1452793716
        },
        "status": "OK",
        "untrusted": false
    }
}
```

## Response Parameters

| Field                          | Type         | Description                     |
| ------------------------------ | ------------ | ------------------------------- |
| result.block\_header.height    | unsigned int | Block height (echoed back)      |
| result.block\_header.hash      | string       | Block hash at this height       |
| result.block\_header.timestamp | unsigned int | Unix timestamp                  |
| result.block\_header.reward    | unsigned int | Coinbase reward in atomic units |

## Use Cases

* Resolving header metadata from a known height
* Block-by-block iteration in indexers
* Computing block-time intervals

## Error Handling

| Status Code | Error Message     | Cause                                                |
| ----------- | ----------------- | ---------------------------------------------------- |
| 403         | Forbidden         | Missing or invalid `<ACCESS-TOKEN>`                  |
| -32602      | Invalid params    | Request parameters are missing or malformed          |
| -32601      | Method not found  | Method does not exist or is not enabled on this node |
| 429         | Too Many Requests | Rate limit exceeded for your plan                    |

## SDK Integration

{% tabs %}
{% tab title="monero-javascript" %}
```javascript
import monerojs from 'monero-javascript';

const daemon = await monerojs.connectToDaemonRpc('https://go.getblock.io/<ACCESS-TOKEN>/');

// Most methods are exposed directly on the daemon client. For raw access:
const result = await daemon.getDaemonConnection().sendJsonRequest('get_block_header_by_height', {"height": 912345});
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
result = backend.raw_request('get_block_header_by_height', {"height": 912345})
print(result)
```
{% endtab %}
{% endtabs %}
