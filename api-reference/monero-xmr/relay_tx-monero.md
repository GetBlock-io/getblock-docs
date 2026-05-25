# relay\_tx monero

This method instructs the node to relay one or more transactions (identified by hash) to its peers. The transactions must already be in the node's mempool.

## Parameters

| Parameter | Type            | Required | Description                               |
| --------- | --------------- | -------- | ----------------------------------------- |
| txids     | array of string | Yes      | List of transaction hashes (hex) to relay |

## Request Example

{% tabs %}
{% tab title="cURL" %}
```bash
curl --location --request POST 'https://go.getblock.io/<ACCESS-TOKEN>/' \
--header 'Content-Type: application/json' \
--data-raw '{
    "jsonrpc": "2.0",
    "method": "relay_tx",
    "params": {
        "txids": [
            "d6e48158472848e6687173a91ae6eebfa3e1d778e65252ee99d7515d63090408"
        ]
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
    "method": "relay_tx",
    "params": {
        "txids": [
            "d6e48158472848e6687173a91ae6eebfa3e1d778e65252ee99d7515d63090408"
        ]
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
    "method": "relay_tx",
    "params": {
        "txids": [
            "d6e48158472848e6687173a91ae6eebfa3e1d778e65252ee99d7515d63090408"
        ]
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
        "method": "relay_tx",
        "params": {
                "txids": [
                        "d6e48158472848e6687173a91ae6eebfa3e1d778e65252ee99d7515d63090408"
                ]
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
        "status": "OK"
    }
}
```
{% endcode %}

## Response Parameters

| Field         | Type   | Description                 |
| ------------- | ------ | --------------------------- |
| result.status | string | `OK` if relay was attempted |

## Use Cases

* Re-broadcasting transactions stuck in the mempool
* Mining-pool operations re-relaying after upstream issues

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
const result = await daemon.getDaemonConnection().sendJsonRequest('relay_tx', {"txids": ["d6e48158472848e6687173a91ae6eebfa3e1d778e65252ee99d7515d63090408"]});
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
result = backend.raw_request('relay_tx', {"txids": ["d6e48158472848e6687173a91ae6eebfa3e1d778e65252ee99d7515d63090408"]})
print(result)
```
{% endtab %}
{% endtabs %}
