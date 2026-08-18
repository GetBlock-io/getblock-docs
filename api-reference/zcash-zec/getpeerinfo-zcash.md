---
description: >-
  Example code for the getpeerinfo JSON-RPC method. Сomplete guide on how to use
  the getpeerinfo JSON-RPC method in GetBlock.io Web3 documentation.
---

# getpeerinfo - Zcash

This method returns detailed information about each connected peer. Post-Zebra 3.0.0, the response was extended with `subver`, `version`, `services`, `lastrecv`, `banscore`, and `connection_state` fields — bringing parity with Bitcoin Core's peer info surface and enabling detailed network diagnostics.

## Parameters

This method takes no parameters.

## Request

{% tabs %}
{% tab title="cURL" %}
{% code overflow="wrap" %}
```bash
curl --location --request POST 'https://shared.eu-central-1.getblock.io/<ACCESS-TOKEN>/' \
--header 'Content-Type: application/json' \
--data-raw '{
    "jsonrpc": "2.0",
    "method": "getpeerinfo",
    "params": [],
    "id": "getblock.io"
}'
```
{% endcode %}
{% endtab %}

{% tab title="Axios" %}
{% code title="example.js" %}
```javascript
const axios = require('axios');

const response = await axios.post('https://shared.eu-central-1.getblock.io/<ACCESS-TOKEN>/', {
    jsonrpc: '2.0',
    method: 'getpeerinfo',
    params: [],
    id: 'getblock.io'
}, {
    headers: { 'Content-Type': 'application/json' }
});

console.log(response.data.result);
```
{% endcode %}
{% endtab %}

{% tab title="Request" %}
{% code title="example.py" %}
```python
import requests

response = requests.post(
    'https://shared.eu-central-1.getblock.io/<ACCESS-TOKEN>/',
    headers={'Content-Type': 'application/json'},
    json={
        'jsonrpc': '2.0',
        'method': 'getpeerinfo',
        'params': [],
        'id': 'getblock.io'
    }
)

print(response.json())
```
{% endcode %}
{% endtab %}

{% tab title="Rust" %}
{% code title="example.rs" %}
```rust
use reqwest::Client;
use serde_json::{json, Value};

#[tokio::main]
async fn main() -> Result<(), Box<dyn std::error::Error>> {
    let client = Client::new();
    
    let response = client
        .post("https://shared.eu-central-1.getblock.io/<ACCESS-TOKEN>/")
        .header("Content-Type", "application/json")
        .json(&json!({
            "jsonrpc": "2.0",
            "method": "getpeerinfo",
            "params": [],
            "id": "getblock.io"
        }))
        .send()
        .await?
        .json::<Value>()
        .await?;
    
    println!("Result: {}", response["result"]);
    Ok(())
}
```
{% endcode %}
{% endtab %}
{% endtabs %}

## Response

```json
{
    "jsonrpc": "2.0",
    "id": "getblock.io",
    "result": [
        {
            "addr": "213.136.68.237:8233",
            "services": "0000000000000001",
            "lastrecv": 1784671886,
            "inbound": false,
            "banscore": 0,
            "subver": "/Zebra:5.1.0/",
            "version": 170150,
            "connection_state": "connected",
            "pingtime": 0.031849273
        },
         {
            "addr": "47.75.194.174:8233",
            "services": "0000000000000001",
            "lastrecv": 1784671855,
            "inbound": false,
            "banscore": 0,
            "subver": "/Zakura:1.0.1/",
            "version": 170160,
            "connection_state": "connected",
            "pingtime": 0.183128632
        }
    ]
}
```

## Response Parameters

| Parameter          | Type    | Description                                                  |
| ------------------ | ------- | ------------------------------------------------------------ |
| `id`               | integer | Peer identifier (per-node local ID)                          |
| `addr`             | string  | IP and port of the peer                                      |
| `addrbind`         | string  | Local bind address for this connection                       |
| `addrlocal`        | string  | Local address as seen by the peer                            |
| `services`         | string  | Hex-encoded service bitmask (Zebra 3.0.0+)                   |
| `lastsend`         | integer | Unix timestamp of the last message sent                      |
| `lastrecv`         | integer | Unix timestamp of the last message received (Zebra 3.0.0+)   |
| `bytessent`        | integer | Total bytes sent to this peer                                |
| `bytesrecv`        | integer | Total bytes received from this peer                          |
| `conntime`         | integer | Unix timestamp of connection establishment                   |
| `pingtime`         | number  | Round-trip ping time in seconds                              |
| `version`          | integer | Peer's protocol version (Zebra 3.0.0+)                       |
| `subver`           | string  | Peer's user agent string (Zebra 3.0.0+)                      |
| `inbound`          | boolean | Whether this is an inbound connection                        |
| `startingheight`   | integer | Peer's advertised starting height                            |
| `banscore`         | integer | Misbehavior score (Zebra 3.0.0+)                             |
| `synced_headers`   | integer | Last header height synced with this peer                     |
| `synced_blocks`    | integer | Last block height synced with this peer                      |
| `connection_state` | string  | Connection state (`Ready`, `Handshake`, etc.) (Zebra 3.0.0+) |

## Use Cases

* **Peer Health Diagnostics**: Detect stalled peers via `lastrecv` gaps and elevated `pingtime`
* **Client Diversity Monitoring**: Track `subver` distribution to measure network client diversity
* **Ban Score Alerts**: Alert on peers approaching the ban threshold (typically 100)
* **Connection State Debugging**: Use `connection_state` to diagnose handshake failures

## Error Handling

| Error Code | Message        | Description                         |
| ---------- | -------------- | ----------------------------------- |
| -32603     | Internal error | Node failed to enumerate peer state |
