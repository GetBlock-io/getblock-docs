---
description: >-
  Example code for the offchain_localStorageGet JSON-RPC method. Complete guide
  on how to use offchain_localStorageGet JSON-RPC in GetBlock Web3
  documentation.
---

# offchain\_localStorageGet - Midnight

This method reads a value from the node's off-chain worker local storage. Off-chain workers can persist data here for use across runs. Local storage is per-node and not part of the on-chain state.

## Parameters

| Parameter | Type   | Required | Description                                                             |
| --------- | ------ | -------- | ----------------------------------------------------------------------- |
| kind      | string | Yes      | Storage kind: `PERSISTENT` (durable) or `LOCAL` (transient per-session) |
| key       | string | Yes      | Hex-encoded storage key                                                 |

## Request Example

{% tabs %}
{% tab title="cURL" %}
```bash
curl --location --request POST 'https://go.getblock.io/<ACCESS-TOKEN>/' \
--header 'Content-Type: application/json' \
--data-raw '{
    "jsonrpc": "2.0",
    "method": "offchain_localStorageGet",
    "params": [
        "PERSISTENT",
        "0x6d795f6b6579"
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
    "method": "offchain_localStorageGet",
    "params": [
        "PERSISTENT",
        "0x6d795f6b6579"
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
    "method": "offchain_localStorageGet",
    "params": [
        "PERSISTENT",
        "0x6d795f6b6579"
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
        "method": "offchain_localStorageGet",
        "params": [
                "PERSISTENT",
                "0x6d795f6b6579"
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
    "result": "0x68656c6c6f"
}
```
{% endcode %}

## Response Parameters

| Field  | Type           | Description                                               |
| ------ | -------------- | --------------------------------------------------------- |
| result | string \| null | Hex-encoded stored value, or `null` if the key is not set |

## Use Cases

* Inspecting off-chain worker state during development
* Reading cached data persisted by an off-chain worker
* Debugging off-chain worker behaviour

## Error Handling

| Status Code | Error Message     | Cause                                       |
| ----------- | ----------------- | ------------------------------------------- |
| 403         | Forbidden         | Missing or invalid `<ACCESS-TOKEN>`         |
| -32602      | Invalid params    | Request parameters are missing or malformed |
| 429         | Too Many Requests | Rate limit exceeded for your plan           |

## SDK Integration

{% tabs %}
{% tab title="@polkadot/api (JavaScript)" %}
```javascript
import { ApiPromise, WsProvider } from '@polkadot/api';

const wsProvider = new WsProvider('wss://go.getblock.io/<ACCESS-TOKEN>/');
const api = await ApiPromise.create({ provider: wsProvider });

// Call via the typed API:
const result = await api.rpc.offchain.localStorageGet("PERSISTENT", "0x6d795f6b6579");
console.log(result.toJSON());

// Or call the raw JSON-RPC method:
const raw = await api.rpc.rpc.methods();
// const raw = await (api as any)._rpcCore.provider.send('offchain_localStorageGet', ["PERSISTENT", "0x6d795f6b6579"]);
```
{% endtab %}

{% tab title="substrate-interface (Python)" %}
```python
from substrateinterface import SubstrateInterface

substrate = SubstrateInterface(url='wss://go.getblock.io/<ACCESS-TOKEN>/')

# Generic RPC call:
result = substrate.rpc_request('offchain_localStorageGet', ["PERSISTENT", "0x6d795f6b6579"])
print(result)
```
{% endtab %}
{% endtabs %}
