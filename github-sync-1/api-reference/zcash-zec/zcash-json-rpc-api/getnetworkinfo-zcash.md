---
description: >-
  Example code for the getnetworkinfo JSON-RPC method. Complete guide on how to
  use the getnetworkinfo JSON-RPC method in GetBlock.io Web3 documentation.
---

# getnetworkinfo - Zcash

This method returns network status information for the node, including its protocol version and connection count.

{% hint style="info" %}
Zebra returns a subset of Bitcoin Core's `getnetworkinfo`. The per-direction counts `connections_in` and `connections_out`, and the `localrelay`, `networkactive`, and `incrementalfee` fields, are not present.
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
    "method": "getnetworkinfo",
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
    method: 'getnetworkinfo',
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
        'method': 'getnetworkinfo',
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
            "method": "getnetworkinfo",
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
    "result": {
        "version": 6030000,
        "subversion": "/Zebra:6.3.0/",
        "protocolversion": 170160,
        "localservices": "0000000000000001",
        "timeoffset": 0,
        "connections": 69,
        "networks": [
            {
                "name": "ipv4",
                "limited": false,
                "reachable": true,
                "proxy": "",
                "proxy_randomize_credentials": false
            },
            {
                "name": "ipv6",
                "limited": false,
                "reachable": true,
                "proxy": "",
                "proxy_randomize_credentials": false
            },
            {
                "name": "onion",
                "limited": false,
                "reachable": false,
                "proxy": "",
                "proxy_randomize_credentials": false
            }
        ],
        "relayfee": 1e-06,
        "localaddresses": [],
        "warnings": ""
    },
    "id": "getblock.io"
}
```

## Response Parameters

| Parameter         | Type            | Description                                  |
| ----------------- | --------------- | -------------------------------------------- |
| `version`         | integer         | Zebra numeric version                        |
| `subversion`      | string          | User agent string sent to peers              |
| `protocolversion` | integer         | P2P protocol version                         |
| `localservices`   | string          | Hex-encoded services supported by this node  |
| `timeoffset`      | integer         | Median time offset in seconds vs peer clocks |
| `connections`     | integer         | Total active peer connections                |
| `networks`        | array of object | Per-network (IPv4, IPv6, Tor) status objects |
| `relayfee`        | number          | Minimum relay fee in ZEC/kB                  |
| `localaddresses`  | array of object | Locally-advertised addresses                 |
| `warnings`        | string          | Any node warnings (empty if none)            |

## Use Cases

* **Health Checks**: Read `connections` to confirm the node has peers
* **Version Checks**: Read `subversion` and `protocolversion` to identify the node
* **Fee Floors**: Read `relayfee` for the minimum relay fee in ZEC per kilobyte
* **Node Warnings Monitoring**: Poll the `warnings` field to detect network-layer issues that don't stop the node

## Error Handling

| Error Code | Message          | Description                                                                       |
| ---------- | ---------------- | --------------------------------------------------------------------------------- |
| -32601     | Method not found | Node does not expose `getnetworkinfo` |
| -32603     | Internal error   | Node failed to compile the network info payload                                   |
