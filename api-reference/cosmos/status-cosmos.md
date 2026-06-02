---
description: >-
  Example code for the status JSON-RPC method. Complete guide on how to use
  status JSON-RPC in GetBlock Web3 documentation.
---

# status - Cosmos

Returns comprehensive node status — node info, public key, latest block hash, app hash, block height, and block time. This is the single most useful method for confirming connectivity and sync status against a Cosmos Hub node.

## Parameters

* None

## Request Example

{% tabs %}
{% tab title="cURL" %}
```bash
# JSON-RPC over HTTP POST (canonical)
curl --location --request POST 'https://go.getblock.io/<ACCESS-TOKEN>/' \
--header 'Content-Type: application/json' \
--data-raw '{
    "jsonrpc": "2.0",
    "method": "status",
    "params": [],
    "id": "getblock.io"
}'

# URI over HTTP GET (alternate, for simple methods)
curl --location --request GET 'https://go.getblock.io/<ACCESS-TOKEN>/status'
```
{% endtab %}

{% tab title="JavaScript (Axios)" %}
```javascript
import axios from 'axios';

const data = JSON.stringify({
    "jsonrpc": "2.0",
    "method": "status",
    "params": [],
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
    "method": "status",
    "params": [],
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
        "method": "status",
        "params": [],
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
    "jsonrpc": "2.0",
    "id": "getblock.io",
    "result": {
        "node_info": {
            "protocol_version": {
                "p2p": "8",
                "block": "11",
                "app": "0"
            },
            "id": "b93270b358a72a2db30089f3856475bb1f918d6d",
            "listen_addr": "tcp://0.0.0.0:26656",
            "network": "cosmoshub-4",
            "version": "v0.37.4",
            "channels": "40202122233038606100",
            "moniker": "aib-hub-node",
            "other": {
                "tx_index": "on",
                "rpc_address": "tcp://0.0.0.0:26657"
            }
        },
        "sync_info": {
            "latest_block_hash": "6E2A2A6FE7A5B4C2D4E6F8A0B2C4D6E8F0A2C4E6F8B0D2E4F6A8C0E2F4B6D8E0",
            "latest_app_hash": "F8C44C8E2E4E8C0F2B7A8E6D4C2E0F1A3B5C7D9E1F2A4B6C8D0E2F4A6B8C0D2E",
            "latest_block_height": "23456789",
            "latest_block_time": "2026-05-26T13:42:18.5273Z",
            "earliest_block_height": "5200791",
            "earliest_block_time": "2021-09-29T16:55:20.456Z",
            "catching_up": false
        },
        "validator_info": {
            "address": "8E2B7A8E6D4C2E0F1A3B5C7D9E1F2A4B6C8D0E2F",
            "pub_key": {
                "type": "tendermint/PubKeyEd25519",
                "value": "AAAA..."
            },
            "voting_power": "0"
        }
    }
}
```
{% endcode %}

## Response Parameters

| Field                                     | Type    | Description                                                                        |
| ----------------------------------------- | ------- | ---------------------------------------------------------------------------------- |
| result.node\_info.network                 | string  | Chain ID — should be `cosmoshub-4` for Cosmos Hub mainnet                          |
| result.node\_info.version                 | string  | CometBFT software version                                                          |
| result.node\_info.moniker                 | string  | Human-readable node name                                                           |
| result.sync\_info.latest\_block\_height   | string  | Most recent block height (decimal string)                                          |
| result.sync\_info.latest\_block\_hash     | string  | Hash of the most recent block                                                      |
| result.sync\_info.latest\_block\_time     | string  | ISO 8601 timestamp of the most recent block                                        |
| result.sync\_info.catching\_up            | boolean | `true` if the node is still syncing, `false` if caught up                          |
| result.sync\_info.earliest\_block\_height | string  | Lowest block height stored locally — useful for detecting archive vs pruning nodes |
| result.validator\_info.voting\_power      | string  | Voting power if this node is also a validator                                      |

## Use Cases

* Confirming network connectivity to Cosmos Hub specifically (`network` = `cosmoshub-4`)
* Sync-status monitoring in failover-aware client setups
* Detecting archive vs pruning nodes via `earliest_block_height`
* Single-call comprehensive health checks

## Error Handling

| Status Code | Error Message     | Cause                                          |
| ----------- | ----------------- | ---------------------------------------------- |
| 403         | Forbidden         | Missing or invalid `<ACCESS-TOKEN>`            |
| -32602      | Invalid params    | Request parameters are missing or malformed    |
| -32603      | Internal error    | Server-side error while processing the request |
| 429         | Too Many Requests | Rate limit exceeded for your plan              |

## SDK Integration

{% tabs %}
{% tab title="CosmJS" %}
```javascript
import { Tendermint37Client, HttpClient } from '@cosmjs/tendermint-rpc';

const httpClient = new HttpClient('https://go.getblock.io/<ACCESS-TOKEN>/');
const client = await Tendermint37Client.create(httpClient);

// CosmJS exposes typed methods on the client (e.g. client.status(), client.block()).
// For raw access to any RPC method:
const result = await httpClient.execute({"jsonrpc": "2.0", "id": "getblock.io", "method": "status", "params": []});
console.log(result);
```
{% endtab %}

{% tab title="cosmpy / requests" %}
```python
# cosmpy wraps Cosmos REST / gRPC. For raw CometBFT RPC access, use requests.
import requests

response = requests.post('https://go.getblock.io/<ACCESS-TOKEN>/', json={
    'jsonrpc': '2.0',
    'id': 'getblock.io',
    'method': 'status',
    'params': []
}})
print(response.json())
```
{% endtab %}
{% endtabs %}
