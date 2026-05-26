---
description: >-
  Example code for the get_connections JSON-RPC method. Complete guide on how to
  use get_connections JSON-RPC in GetBlock Web3 documentation.
---

# get\_connections - Monero

This method returns the list of current P2P connections of the node along with their statistics.

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
    "method": "get_connections",
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
    "method": "get_connections",
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
    "method": "get_connections",
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
        "method": "get_connections",
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
        "connections": [
            {
                "address": "192.0.2.1:18080",
                "address_type": 1,
                "avg_download": 1,
                "avg_upload": 5,
                "connection_id": "abc123",
                "current_download": 0,
                "current_upload": 0,
                "height": 3194582,
                "host": "192.0.2.1",
                "incoming": false,
                "ip": "192.0.2.1",
                "live_time": 12345,
                "local_ip": false,
                "localhost": false,
                "peer_id": "abcdef0123456789",
                "port": "18080",
                "pruning_seed": 0,
                "recv_count": 1000,
                "recv_idle_time": 0,
                "rpc_credits_per_hash": 0,
                "rpc_port": 0,
                "send_count": 1000,
                "send_idle_time": 0,
                "state": "normal",
                "support_flags": 1
            }
        ],
        "status": "OK"
    }
}
```
{% endcode %}

## Response Parameters

| Field                          | Type         | Description                            |
| ------------------------------ | ------------ | -------------------------------------- |
| result.connections             | array        | Array of active P2P connection records |
| result.connections\[].address  | string       | Remote peer address                    |
| result.connections\[].height   | unsigned int | Peer's reported chain height           |
| result.connections\[].state    | string       | Connection state                       |
| result.connections\[].incoming | boolean      | Whether the connection is inbound      |

## Use Cases

* Inspecting the node's peer set
* Network analytics and topology research
* Detecting connection-level anomalies

## Error Handling

| Status Code | Error Message     | Cause                                       |
| ----------- | ----------------- | ------------------------------------------- |
| 404         | Not found         | Missing or invalid `<ACCESS-TOKEN>`         |
| -32602      | Invalid params    | Request parameters are missing or malformed |
| 429         | Too Many Requests | Rate limit exceeded for your plan           |

## SDK Integration

{% tabs %}
{% tab title="monero-javascript" %}
```javascript
import monerojs from 'monero-javascript';

const daemon = await monerojs.connectToDaemonRpc('https://go.getblock.io/<ACCESS-TOKEN>/');

// Most methods are exposed directly on the daemon client. For raw access:
const result = await daemon.getDaemonConnection().sendJsonRequest('get_connections', {});
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
result = backend.raw_request('get_connections', {})
print(result)
```
{% endtab %}
{% endtabs %}
