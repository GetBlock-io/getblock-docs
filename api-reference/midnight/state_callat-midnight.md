---
description: >-
  Example code for the state_callAt JSON-RPC method. Complete guide on how to
  use state_callAt JSON-RPC in GetBlock Web3 documentation.
---

# state\_callAt - Midnight

This method executes a runtime API call at a specific historical block. Same semantics as `state_call`, but at a chosen block hash.

## Parameters

| Parameter | Type   | Required | Description                         |
| --------- | ------ | -------- | ----------------------------------- |
| name      | string | Yes      | Runtime API method name             |
| bytes     | string | Yes      | SCALE-encoded call parameters (hex) |
| hash      | string | Yes      | Block hash at which to execute      |

## Request Example

{% tabs %}
{% tab title="cURL" %}
```bash
curl --location --request POST 'https://go.getblock.io/<ACCESS-TOKEN>/' \
--header 'Content-Type: application/json' \
--data-raw '{
    "jsonrpc": "2.0",
    "method": "state_callAt",
    "params": [
        "Core_version",
        "0x",
        "0x87f5c08ba8334e4ee1642ff9646e94dd46080b71fff03714be8b36f22e481b95"
    ],
    "id": "getblock.io"
}'
```
{% endtab %}

{% tab title="JavaScript (Axios)" %}
```javascript
import axios from 'axios';

const data = JSON.stringify({
    "jsonrpc": "2.0",
    "method": "state_callAt",
    "params": [
        "Core_version",
        "0x",
        "0x87f5c08ba8334e4ee1642ff9646e94dd46080b71fff03714be8b36f22e481b95"
    ],
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
    "method": "state_callAt",
    "params": [
        "Core_version",
        "0x",
        "0x87f5c08ba8334e4ee1642ff9646e94dd46080b71fff03714be8b36f22e481b95"
    ],
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
        "method": "state_callAt",
        "params": [
                "Core_version",
                "0x",
                "0x87f5c08ba8334e4ee1642ff9646e94dd46080b71fff03714be8b36f22e481b95"
        ],
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
    "result": "0x206d69646e696768742060\u2026"
}
```
{% endcode %}

## Response Parameters

| Field  | Type   | Description                      |
| ------ | ------ | -------------------------------- |
| result | string | SCALE-encoded return value (hex) |

## Use Cases

* Reading runtime state at historical heights
* Replaying typed queries across blocks
* Auditing state changes between block heights

## Error Handling

| Status Code | Error Message     | Cause                                                         |
| ----------- | ----------------- | ------------------------------------------------------------- |
| 403         | Forbidden         | Missing or invalid `<ACCESS-TOKEN>`                           |
| -32602      | Invalid params    | Request parameters are missing or malformed                   |
| -32601      | Method not found  | The method is not enabled on this node or has been deprecated |
| 429         | Too Many Requests | Rate limit exceeded for your plan                             |

## SDK Integration

{% tabs %}
{% tab title="@polkadot/api (JavaScript)" %}
```javascript
import { ApiPromise, WsProvider } from '@polkadot/api';

const wsProvider = new WsProvider('wss://go.getblock.io/<ACCESS-TOKEN>/');
const api = await ApiPromise.create({ provider: wsProvider });

// Call via the typed API:
const result = await api.rpc.state.callAt("Core_version", "0x", "0x87f5c08ba8334e4ee1642ff9646e94dd46080b71fff03714be8b36f22e481b95");
console.log(result.toJSON());

// Or call the raw JSON-RPC method:
const raw = await api.rpc.rpc.methods();
// const raw = await (api as any)._rpcCore.provider.send('state_callAt', ["Core_version", "0x", "0x87f5c08ba8334e4ee1642ff9646e94dd46080b71fff03714be8b36f22e481b95"]);
```
{% endtab %}

{% tab title="substrate-interface (Python)" %}
```python
from substrateinterface import SubstrateInterface

substrate = SubstrateInterface(url='wss://go.getblock.io/<ACCESS-TOKEN>/')

# Generic RPC call:
result = substrate.rpc_request('state_callAt', ["Core_version", "0x", "0x87f5c08ba8334e4ee1642ff9646e94dd46080b71fff03714be8b36f22e481b95"])
print(result)
```
{% endtab %}
{% endtabs %}
