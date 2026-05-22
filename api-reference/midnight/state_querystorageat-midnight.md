---
description: >-
  Example code for the state_queryStorageAt JSON-RPC method. Complete guide on
  how to use state_queryStorageAt JSON-RPC in GetBlock Web3 documentation.
---

# state\_queryStorageAt - Midnight

This method queries the values of multiple storage keys at a specific block in a single call. More efficient than calling `state_getStorageAt` once per key.

## Parameters

| Parameter | Type            | Required | Description                                                |
| --------- | --------------- | -------- | ---------------------------------------------------------- |
| keys      | array of string | Yes      | Array of hex-encoded storage keys                          |
| hash      | string          | No       | Block hash to query at. Defaults to the current best block |

## Request Example

{% tabs %}
{% tab title="cURL" %}
```bash
curl --location --request POST 'https://go.getblock.io/<ACCESS-TOKEN>/' \
--header 'Content-Type: application/json' \
--data-raw '{
    "jsonrpc": "2.0",
    "method": "state_queryStorageAt",
    "params": [
        [
            "0x26aa394eea5630e07c48ae0c9558cef7b99d880ec681799c0cf30e8886371da9"
        ]
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
    "method": "state_queryStorageAt",
    "params": [
        [
            "0x26aa394eea5630e07c48ae0c9558cef7b99d880ec681799c0cf30e8886371da9"
        ]
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
    "method": "state_queryStorageAt",
    "params": [
        [
            "0x26aa394eea5630e07c48ae0c9558cef7b99d880ec681799c0cf30e8886371da9"
        ]
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
        "method": "state_queryStorageAt",
        "params": [
                [
                        "0x26aa394eea5630e07c48ae0c9558cef7b99d880ec681799c0cf30e8886371da9"
                ]
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

```json
{
    "jsonrpc": "2.0",
    "result": [
        {
            "block": "0xfaac1c5cd0b3d56d5f33382691a6c269d5000bbabf1a61df7fa03194b277fbbf",
            "changes": [
                [
                    "0x26aa394eea5630e07c48ae0c9558cef7b99d880ec681799c0cf30e8886371da9",
                    null
                ]
            ]
        }
    ],
    "id": "getblock.io"
}
```

## Response Parameters

| Field             | Type            | Description                                                          |
| ----------------- | --------------- | -------------------------------------------------------------------- |
| result            | array of object | Array of one snapshot per requested block (one entry by default)     |
| result\[].block   | string          | Block hash for this snapshot                                         |
| result\[].changes | array           | Array of `[key, value]` pairs; `value` is `null` if the key is unset |

## Use Cases

* Batched state reads to reduce round-trips
* Indexers that need many keys at the same block height
* Atomic snapshots across multiple storage items

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
const result = await api.rpc.state.queryStorageAt(["0x26aa394eea5630e07c48ae0c9558cef7b99d880ec681799c0cf30e8886371da9"]);
console.log(result.toJSON());

// Or call the raw JSON-RPC method:
const raw = await api.rpc.rpc.methods();
// const raw = await (api as any)._rpcCore.provider.send('state_queryStorageAt', [["0x26aa394eea5630e07c48ae0c9558cef7b99d880ec681799c0cf30e8886371da9"]]);
```
{% endtab %}

{% tab title="substrate-interface (Python)" %}
```python
from substrateinterface import SubstrateInterface

substrate = SubstrateInterface(url='wss://go.getblock.io/<ACCESS-TOKEN>/')

# Generic RPC call:
result = substrate.rpc_request('state_queryStorageAt', [["0x26aa394eea5630e07c48ae0c9558cef7b99d880ec681799c0cf30e8886371da9"]])
print(result)
```
{% endtab %}
{% endtabs %}
