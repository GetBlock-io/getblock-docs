---
description: >-
  Example code for the state_getKeysPaged JSON-RPC method. Complete guide on how
  to use state_getKeysPaged JSON-RPC in GetBlock Web3 documentation.
---

# state\_getKeysPaged - Midnight

This method returns paged storage keys with an optional prefix, allowing efficient iteration over large storage maps. Use the `startKey` parameter to continue from a previous page.

## Parameters

| Parameter | Type    | Required | Description                                                    |
| --------- | ------- | -------- | -------------------------------------------------------------- |
| prefix    | string  | Yes      | Hex-encoded storage key prefix                                 |
| count     | integer | Yes      | Maximum number of keys to return                               |
| startKey  | string  | No       | Hex-encoded key to start from (exclusive). Used for pagination |
| hash      | string  | No       | Block hash to query at. Defaults to the current best block     |

## Request Example

{% tabs %}
{% tab title="cURL" %}
```bash
curl --location --request POST 'https://go.getblock.io/<ACCESS-TOKEN>/' \
--header 'Content-Type: application/json' \
--data-raw '{
    "jsonrpc": "2.0",
    "method": "state_getKeysPaged",
    "params": [
        "0x26aa394eea5630e07c48ae0c9558cef7",
        100
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
    "method": "state_getKeysPaged",
    "params": [
        "0x26aa394eea5630e07c48ae0c9558cef7",
        100
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
    "method": "state_getKeysPaged",
    "params": [
        "0x26aa394eea5630e07c48ae0c9558cef7",
        100
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
        "method": "state_getKeysPaged",
        "params": [
                "0x26aa394eea5630e07c48ae0c9558cef7",
                100
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
    "result": [
        "0x26aa394eea5630e07c48ae0c9558cef7b99d880ec681799c0cf30e8886371da9",
        "0x26aa394eea5630e07c48ae0c9558cef78a42f33323cb5ced3b44dd825fda9fcc"
    ]
}
```
{% endcode %}

## Response Parameters

| Field  | Type            | Description                       |
| ------ | --------------- | --------------------------------- |
| result | array of string | Array of hex-encoded storage keys |

## Use Cases

* Iterating large storage maps without overwhelming the node
* Snapshot exporters and indexers
* Building custom state pruning tools

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
const result = await api.rpc.state.getKeysPaged("0x26aa394eea5630e07c48ae0c9558cef7", 100);
console.log(result.toJSON());

// Or call the raw JSON-RPC method:
const raw = await api.rpc.rpc.methods();
// const raw = await (api as any)._rpcCore.provider.send('state_getKeysPaged', ["0x26aa394eea5630e07c48ae0c9558cef7", 100]);
```
{% endtab %}

{% tab title="substrate-interface (Python)" %}
```python
from substrateinterface import SubstrateInterface

substrate = SubstrateInterface(url='wss://go.getblock.io/<ACCESS-TOKEN>/')

# Generic RPC call:
result = substrate.rpc_request('state_getKeysPaged', ["0x26aa394eea5630e07c48ae0c9558cef7", 100])
print(result)
```
{% endtab %}
{% endtabs %}
