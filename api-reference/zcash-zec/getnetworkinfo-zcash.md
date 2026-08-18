---
description: >-
  Example code for the getnetworkinfo JSON-RPC method. Сomplete guide on how to
  use the getnetworkinfo JSON-RPC method in GetBlock.io Web3 documentation.
---

# getnetworkinfo - Zcash

This method returns detailed network status information: peer counts by direction, protocol version, active listening addresses, and negotiated services. Introduced in Zebra 3.0.0 (January 2026), it provides the rich network-layer diagnostics that Bitcoin Core-family tooling expects and that were previously unavailable in Zebra.

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
    "id": "getblock.io",
    "result": {
        "version": 6000000,
        "subversion": "/Zebra:6.0.0/",
        "protocolversion": 170160,
        "localservices": "0000000000000001",
        "timeoffset": 0,
        "connections": 41,
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
        "relayfee": 1e-6,
        "localaddresses": [],
        "warnings": ""
    }
}
```

## Response Parameters

| Parameter         | Type            | Description                                  |
| ----------------- | --------------- | -------------------------------------------- |
| `version`         | integer         | Zebra numeric version                        |
| `subversion`      | string          | User agent string sent to peers              |
| `protocolversion` | integer         | P2P protocol version                         |
| `localservices`   | string          | Hex-encoded services supported by this node  |
| `localrelay`      | boolean         | Whether the node relays transactions         |
| `timeoffset`      | integer         | Median time offset in seconds vs peer clocks |
| `connections`     | integer         | Total active peer connections                |
| `connections_in`  | integer         | Inbound peer connections                     |
| `connections_out` | integer         | Outbound peer connections                    |
| `networkactive`   | boolean         | Whether the P2P network layer is active      |
| `networks`        | array of object | Per-network (IPv4, IPv6, Tor) status objects |
| `relayfee`        | number          | Minimum relay fee in ZEC/kB                  |
| `incrementalfee`  | number          | Minimum increment for fee bumping            |
| `localaddresses`  | array of object | Locally-advertised addresses                 |
| `warnings`        | string          | Any node warnings (empty if none)            |

## Use Cases

* **Detailed Network Diagnostics**: Distinguish inbound vs outbound connection issues on a self-hosted node
* **Kubernetes / Load Balancer Health Checks**: Use `networkactive` as a P2P readiness signal for orchestration
* **Fee Estimation**: Read `relayfee` and `incrementalfee` for fee-bumping logic on stuck transactions
* **Node Warnings Monitoring**: Poll the `warnings` field to detect network-layer issues that don't stop the node

## Error Handling

| Error Code | Message          | Description                                                                       |
| ---------- | ---------------- | --------------------------------------------------------------------------------- |
| -32601     | Method not found | Node runs an older Zebra version (pre-3.0.0) that doesn't expose `getnetworkinfo` |
| -32603     | Internal error   | Node failed to compile the network info payload                                   |
