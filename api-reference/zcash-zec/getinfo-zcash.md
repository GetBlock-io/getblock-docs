---
description: >-
  Example code for the getinfo JSON-RPC method. Сomplete guide on how to use the
  getinfo JSON-RPC method in GetBlock.io Web3 documentation.
---

# getinfo - Zcash

This method returns basic information about the Zebra node handling the request — build version, network protocol version, current best block height, and connection count. It's the standard starting point for any diagnostic or health-check flow against a Zcash endpoint.

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
    "method": "getinfo",
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
    method: 'getinfo',
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
        'method': 'getinfo',
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
            "method": "getinfo",
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

{% code overflow="wrap" %}
```json
{
    "jsonrpc": "2.0",
    "id": "getblock.io",
    "result": {
        "version": 6000000,
        "build": "v6.0.0",
        "subversion": "/Zebra:6.0.0/",
        "protocolversion": 170160,
        "blocks": 3420502,
        "connections": 41,
        "difficulty": 229698549.25032642,
        "testnet": false,
        "paytxfee": 0.0,
        "relayfee": 1e-6,
        "errors": "chain updates have stalled, state height has not increased for 10 minutes. Hint: check your network connection, and your computer clock and time zone",
        "errorstimestamp": 1784662715
    }
}
```
{% endcode %}

## Response Parameters

| Parameter         | Type    | Description                                                          |
| ----------------- | ------- | -------------------------------------------------------------------- |
| `version`         | integer | Zebra numeric version (e.g. `3000000` = 3.0.0)                       |
| `build`           | string  | Zebra semver build string                                            |
| `subversion`      | string  | User agent string sent to peers                                      |
| `protocolversion` | integer | P2P protocol version                                                 |
| `blocks`          | integer | Current best-known chain tip height                                  |
| `connections`     | integer | Number of active peer connections                                    |
| `proxy`           | string  | Proxy in use for outbound connections (empty if none)                |
| `difficulty`      | number  | Current proof-of-work difficulty as a multiple of the minimum        |
| `testnet`         | boolean | `true` if this endpoint serves Testnet, `false` for Mainnet          |
| `paytxfee`        | number  | Wallet fee setting — always `0.0` on Zebra (no wallet functionality) |
| `relayfee`        | number  | Minimum relay fee in ZEC/kB                                          |
| `errors`          | string  | Most recent error message, if any                                    |
| `errorstimestamp` | string  | Timestamp of the most recent error                                   |

## Use Cases

* **Node Health Probe**: Baseline liveness check as part of an endpoint availability monitor
* **Chain Progress Monitoring**: Poll the `blocks` field to detect chain tip advancement
* **Client Version Detection**: Distinguish Zebra endpoints from remaining zcashd fallbacks by inspecting `build` and `subversion`
* **Network Discovery**: Confirm whether an endpoint is serving Mainnet or Testnet via the `testnet` field
* **Connection Diagnostics**: Detect P2P isolation via low `connections` count on a self-hosted node

## Error Handling

| Error Code | Message        | Description                                                              |
| ---------- | -------------- | ------------------------------------------------------------------------ |
| -32603     | Internal error | Node failed to compile the info payload (rare, transient upstream error) |
