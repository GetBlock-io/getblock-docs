# sync\_info monero

This method returns synchronization status for the node, including peers it is syncing from and per-peer progress. Useful for diagnosing sync stalls.

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
    "method": "sync_info",
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
    "method": "sync_info",
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
    "method": "sync_info",
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
        "method": "sync_info",
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
        "credits": 0,
        "height": 3194582,
        "next_needed_pruning_seed": 0,
        "overview": "[]",
        "peers": [
            {
                "info": {
                    "address": "192.0.2.1:18080",
                    "current_download": 0,
                    "current_upload": 0,
                    "height": 3194582,
                    "incoming": false,
                    "ip": 16777343,
                    "live_time": 12345,
                    "peer_id": "abcdef0123456789",
                    "port": "18080",
                    "pruning_seed": 0,
                    "recv_count": 1000,
                    "recv_idle_time": 0,
                    "send_count": 1000,
                    "send_idle_time": 0,
                    "state": "normal",
                    "support_flags": 1
                }
            }
        ],
        "status": "OK",
        "target_height": 0,
        "top_hash": "",
        "untrusted": false
    }
}
```
{% endcode %}

## Response Parameters

| Field                        | Type         | Description                                |
| ---------------------------- | ------------ | ------------------------------------------ |
| result.height                | unsigned int | Current node height                        |
| result.target\_height        | unsigned int | Target sync height (`0` when fully synced) |
| result.peers                 | array        | List of peers and their connection state   |
| result.peers\[].info.address | string       | Peer address                               |
| result.peers\[].info.height  | unsigned int | Peer's reported height                     |
| result.peers\[].info.state   | string       | Connection state with this peer            |

## Use Cases

* Diagnosing sync issues
* Per-peer download/upload monitoring
* Detecting eclipse-attack scenarios in self-hosted setups

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
const result = await daemon.getDaemonConnection().sendJsonRequest('sync_info', {});
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
result = backend.raw_request('sync_info', {})
print(result)
```
{% endtab %}
{% endtabs %}
