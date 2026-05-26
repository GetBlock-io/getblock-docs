---
description: >-
  Example code for the get_info JSON-RPC method. Complete guide on how to use
  get_info JSON-RPC in GetBlock Web3 documentation.
---

# get\_info - Monero

This method returns a comprehensive snapshot of the node's state and the network it's connected to — block height, difficulty, connection counts, sync status, and more. This is the most useful single call for health-checking and diagnostics.

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
    "method": "get_info",
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
    "method": "get_info",
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
    "method": "get_info",
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
        "method": "get_info",
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

```json
{
    "id": "getblock.io",
    "jsonrpc": "2.0",
    "result": {
        "adjusted_time": 1737869732,
        "alt_blocks_count": 2,
        "block_size_limit": 600000,
        "block_size_median": 300000,
        "block_weight_limit": 600000,
        "block_weight_median": 300000,
        "bootstrap_daemon_address": "",
        "busy_syncing": false,
        "credits": 0,
        "cumulative_difficulty": 116197140699118346,
        "cumulative_difficulty_top64": 0,
        "database_size": 114352345088,
        "difficulty": 294558564173,
        "difficulty_top64": 0,
        "free_space": 514427379712,
        "grey_peerlist_size": 4999,
        "height": 3194582,
        "height_without_bootstrap": 3194582,
        "incoming_connections_count": 0,
        "mainnet": true,
        "nettype": "mainnet",
        "offline": false,
        "outgoing_connections_count": 12,
        "rpc_connections_count": 7,
        "stagenet": false,
        "start_time": 1614267442,
        "status": "OK",
        "synchronized": true,
        "target": 120,
        "target_height": 0,
        "testnet": false,
        "top_block_hash": "e22cf75f39ae720e8b71b3d120a5ac03f0db50bba6379e2850975b4859190bc6",
        "top_hash": "",
        "tx_count": 14490301,
        "tx_pool_size": 13,
        "untrusted": false,
        "version": "0.18.3.4-release",
        "was_bootstrap_ever_used": false,
        "white_peerlist_size": 1000,
        "wide_cumulative_difficulty": "0x19c19c\u2026",
        "wide_difficulty": "0x44a5b7\u2026"
    }
}
```

## Response Parameters

| Field                               | Type         | Description                                               |
| ----------------------------------- | ------------ | --------------------------------------------------------- |
| result.height                       | unsigned int | Current chain height                                      |
| result.target\_height               | unsigned int | Height the node is syncing toward (`0` when fully synced) |
| result.difficulty                   | unsigned int | Current network difficulty                                |
| result.tx\_count                    | unsigned int | Cumulative transaction count on chain                     |
| result.tx\_pool\_size               | unsigned int | Number of transactions in the mempool                     |
| result.outgoing\_connections\_count | unsigned int | Outgoing P2P connections                                  |
| result.synchronized                 | boolean      | `true` when the node has caught up with the network       |
| result.mainnet                      | boolean      | `true` for mainnet                                        |
| result.nettype                      | string       | Network type: `mainnet`, `stagenet`, or `testnet`         |
| result.version                      | string       | monerod software version                                  |

## Use Cases

* Comprehensive node health checks
* Network statistics dashboards
* Sync-status monitoring in failover-aware client setups
* Wallet bootstrap calls before submitting transactions

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
const result = await daemon.getDaemonConnection().sendJsonRequest('get_info', {});
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
result = backend.raw_request('get_info', {})
print(result)
```
{% endtab %}
{% endtabs %}
