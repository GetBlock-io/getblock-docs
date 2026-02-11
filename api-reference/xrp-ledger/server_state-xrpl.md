---
description: >-
  Example code for the server_state JSON RPC method. Complete guide to using
  server_state JSON-RPC in the GetBlock Web3 documentation.
---

# server\_state - XRPL

This method asks the server for various machine-readable information about the rippled server's current state.

## Parameters

* None

## Request Example

{% tabs %}
{% tab title="cURL" %}
{% code title="cURL" %}
```bash
curl --location --request POST 'https://xrp.getblock.io/mainnet/' \
--header 'x-api-key: YOUR-API-KEY' \
--header 'Content-Type: application/json' \
--data-raw '{
    "jsonrpc": "2.0",
    "method": "server_state",
    "params": [{}],
    "id": "getblock.io"
}'
```
{% endcode %}
{% endtab %}

{% tab title="JavaScript (Axios)" %}
{% code title="server_state.js" %}
```javascript
const axios = require('axios');

const url = 'https://xrp.getblock.io/mainnet/';
const headers = {
    'x-api-key': 'YOUR-API-KEY',
    'Content-Type': 'application/json'
};

const payload = {
    jsonrpc: '2.0',
    method: 'server_state',
    params: [{}],
    id: 'getblock.io'
};

axios.post(url, payload, { headers })
    .then(response => console.log(response.data))
    .catch(error => console.error(error));
```
{% endcode %}
{% endtab %}

{% tab title="Python" %}
{% code title="server_state.py" %}
```python
import requests

url = "https://xrp.getblock.io/mainnet/"
headers = {
    "x-api-key": "YOUR-API-KEY",
    "Content-Type": "application/json"
}

payload = {
    "jsonrpc": "2.0",
    "method": "server_state",
    "params": [{}],
    "id": "getblock.io"
}

response = requests.post(url, headers=headers, json=payload)
print(response.json())
```
{% endcode %}
{% endtab %}

{% tab title="Rust" %}
{% code title="main.rs" %}
```rust
use reqwest::Client;
use serde_json::json;

#[tokio::main]
async fn main() -> Result<(), Box<dyn std::error::Error>> {
    let client = Client::new();
    
    let payload = json!({
        "jsonrpc": "2.0",
        "method": "server_state",
        "params": [{}],
        "id": "getblock.io"
    });

    let response = client
        .post("https://xrp.getblock.io/mainnet/")
        .header("x-api-key", "YOUR-API-KEY")
        .header("Content-Type", "application/json")
        .json(&payload)
        .send()
        .await?;

    let result: serde_json::Value = response.json().await?;
    println!("{:#?}", result);
    
    Ok(())
}
```
{% endcode %}
{% endtab %}
{% endtabs %}

## Response Example

```json
{
    "result": {
        "state": {
            "build_version": "3.1.0",
            "complete_ledgers": "100120029-102052988",
            "initial_sync_duration_us": "915820835",
            "io_latency_ms": 1,
            "jq_trans_overflow": "0",
            "last_close": {
                "converge_time": 3006,
                "proposers": 34
            },
            "load_base": 256,
            "load_factor": 256,
            "load_factor_fee_escalation": 256,
            "load_factor_fee_queue": 256,
            "load_factor_fee_reference": 256,
            "load_factor_server": 256,
            "network_id": 0,
            "peer_disconnects": "9670",
            "peer_disconnects_resources": "0",
            "peers": 131,
            "ports": [
                {
                    "port": "5005",
                    "protocol": [
                        "http"
                    ]
                },
                {
                    "port": "80",
                    "protocol": [
                        "ws"
                    ]
                },
                {
                    "port": "2459",
                    "protocol": [
                        "peer"
                    ]
                },
                {
                    "port": "50051",
                    "protocol": [
                        "grpc"
                    ]
                }
            ],
            "pubkey_node": "n9KRYW727sNnzqDEi9EKLUiyDMCNPrbMX812jBeHet5BN31Z4N3i",
            "server_state": "full",
            "server_state_duration_us": "664596474942",
            "state_accounting": {
                "connected": {
                    "duration_us": "912677334",
                    "transitions": "2"
                },
                "disconnected": {
                    "duration_us": "1050564",
                    "transitions": "2"
                },
                "full": {
                    "duration_us": "664596474942",
                    "transitions": "1"
                },
                "syncing": {
                    "duration_us": "2092921",
                    "transitions": "1"
                },
                "tracking": {
                    "duration_us": "14",
                    "transitions": "1"
                }
            },
            "time": "2026-Feb-06 00:23:48.280112 UTC",
            "uptime": 665512,
            "validated_ledger": {
                "base_fee": 10,
                "close_time": 823652622,
                "hash": "6879EBDFC4A5089515CD3B490B8AE204D89F7096A5D989388AECEA456C15FFDE",
                "reserve_base": 1000000,
                "reserve_inc": 200000,
                "seq": 102052988
            },
            "validation_quorum": 28
        },
        "status": "success"
    }
}
```

## Response Parameters

| Parameter         | Type    | Description      |
| ----------------- | ------- | ---------------- |
| server\_state     | string  | Current state    |
| load\_factor      | integer | Load factor      |
| validated\_ledger | object  | Latest validated |

| Field             | Type   | Description       |
| ----------------- | ------ | ----------------- |
| state             | object | Server state info |
| build\_version    | string | Server version    |
| complete\_ledgers | string | Available ledgers |

## Use Cases

* Monitor server performance
* Check sync status
* System diagnostics

## Error Handling

| Error Code | Description   |
| ---------- | ------------- |
| noNetwork  | Not connected |

## SDK Integration

{% tabs %}
{% tab title="xrpl.js" %}
{% code title="xrpl.js example" %}
```javascript
const xrpl = require('xrpl');

async function getServerState() {
    const client = new xrpl.Client('wss://xrplcluster.com');
    await client.connect();
    
    const response = await client.request({
        command: 'server_state'
    });
    
    console.log('State:', response.result.state.server_state);
    await client.disconnect();
}

getServerState();
```
{% endcode %}
{% endtab %}

{% tab title="xrpl-py" %}
{% code title="xrpl-py example" %}
```python
from xrpl.clients import JsonRpcClient
from xrpl.models.requests import ServerState

client = JsonRpcClient("https://xrp.getblock.io/mainnet/")
request = ServerState()
response = client.request(request)
print(response.result)
```
{% endcode %}
{% endtab %}
{% endtabs %}
