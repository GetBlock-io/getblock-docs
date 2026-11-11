---
description: >-
  Example code for the getpeerinfo JSON-RPC method. Complete guide on how to use
  the getpeerinfo JSON-RPC method in GetBlock.io Web3 documentation.
---

# getpeerinfo - Zcash

This method returns information about each peer the node is connected to, including its address, advertised services, protocol version, user agent, and connection state.

{% hint style="info" %}
Zebra returns fewer fields per peer than Bitcoin Core. Traffic counters such as `bytessent` and `bytesrecv`, and sync fields such as `synced_blocks` and `startingheight`, are not present.
{% endhint %}

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
    "result": [
        {
            "addr": "168.119.7.215:8233",
            "services": "0000000000000001",
            "lastrecv": 1789744680,
            "inbound": false,
            "banscore": 0,
            "subver": "/Zebra:6.3.0/",
            "version": 170160,
            "connection_state": "connected",
            "pingtime": 0.051483004
        },
        {
            "addr": "65.109.86.46:8233",
            "services": "0000000000000001",
            "lastrecv": 1789744688,
            "inbound": false,
            "banscore": 0,
            "subver": "/Zakura:1.2.0/",
            "version": 170160,
            "connection_state": "connected",
            "pingtime": 0.032725716
        }
    ],
    "id": "getblock.io"
}
```

## Response Parameters

| Parameter          | Type    | Description                                                  |
| ------------------ | ------- | ------------------------------------------------------------ |
| `addr`             | string  | IP and port of the peer                                      |
| `services`         | string  | Hex-encoded service bitmask                   |
| `lastrecv`         | integer | Unix timestamp of the last message received   |
| `pingtime`         | number  | Round-trip ping time in seconds                              |
| `version`          | integer | Peer's protocol version                       |
| `subver`           | string  | Peer's user agent string                      |
| `inbound`          | boolean | Whether this is an inbound connection                        |
| `banscore`         | integer | Misbehavior score                             |
| `connection_state` | string  | Connection state, such as `connected`         |

## Use Cases

* **Peer Health Diagnostics**: Detect stalled peers via `lastrecv` gaps and elevated `pingtime`
* **Client Diversity Monitoring**: Track `subver` distribution to measure network client diversity
* **Ban Score Alerts**: Alert on peers approaching the ban threshold (typically 100)
* **Connection State Debugging**: Use `connection_state` to diagnose handshake failures

## Error Handling

| Error Code | Message        | Description                         |
| ---------- | -------------- | ----------------------------------- |
| -32603     | Internal error | Node failed to enumerate peer state |
