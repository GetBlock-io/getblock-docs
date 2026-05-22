---
description: >-
  Example code for the chain_getRuntimeVersion JSON-RPC method. Complete guide
  on how to use chain_getRuntimeVersion JSON-RPC in GetBlock Web3 documentation.
---

# chain\_getRuntimeVersion - Midnight

This method returns the runtime version metadata at a given block (or the current best block if no hash is provided). Runtime version changes during a runtime upgrade.

## Parameters

| Parameter | Type   | Required | Description                                             |
| --------- | ------ | -------- | ------------------------------------------------------- |
| hash      | string | No       | Block hash to query. Defaults to the current best block |

## Request Example

{% tabs %}
{% tab title="cURL" %}
```bash
curl --location --request POST 'https://go.getblock.io/<ACCESS-TOKEN>/' \
--header 'Content-Type: application/json' \
--data-raw '{
    "jsonrpc": "2.0",
    "method": "chain_getRuntimeVersion",
    "params": [],
    "id": "getblock.io"
}'
```
{% endtab %}

{% tab title="JavaScript (Axios)" %}
```javascript
import axios from 'axios';

const data = JSON.stringify({
    "jsonrpc": "2.0",
    "method": "chain_getRuntimeVersion",
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
    "method": "chain_getRuntimeVersion",
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
        "method": "chain_getRuntimeVersion",
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

```json
{
    "jsonrpc": "2.0",
    "result": {
        "specName": "midnight",
        "implName": "midnight",
        "authoringVersion": 1,
        "specVersion": 22000,
        "implVersion": 0,
        "apis": [
            [
                "0xf78b278be53f454c",
                2
            ],
            [
                "0x279a32908e96410d",
                1
            ],
            [
                "0xbc9d89904f5b923f",
                1
            ],
            [
                "0xdd718d5cc53262d4",
                1
            ],
            [
                "0xa897625b130086bd",
                1
            ],
            [
                "0xfbc577b9d747efd6",
                1
            ],
            [
                "0xdf6acb689907609b",
                5
            ],
            [
                "0x49eaaf1b548a0cb0",
                5
            ],
            [
                "0x393b2599ea57bca2",
                1
            ],
            [
                "0xa0c89371ec56244c",
                1
            ],
            [
                "0xab3c0572291feb8b",
                1
            ],
            [
                "0x2a5e924655399e60",
                1
            ],
            [
                "0x37e397fc7c91f5e4",
                2
            ],
            [
                "0xc3c222f40af6f42b",
                1
            ],
            [
                "0x22a0eaa81c05f0d3",
                1
            ],
            [
                "0x0093e736d7484c30",
                1
            ],
            [
                "0xd9bc84ac3bfb8a0c",
                1
            ],
            [
                "0xd2bc9897eed08f15",
                3
            ],
            [
                "0x40fe3ad401f8959a",
                6
            ],
            [
                "0xed99c5acb25eedf5",
                3
            ],
            [
                "0xc7805fd5ec973a86",
                1
            ],
            [
                "0x19dba14093498f16",
                2
            ],
            [
                "0x7d699da8a672e867",
                5
            ],
            [
                "0x91d5df18b0d2cf58",
                2
            ]
        ],
        "transactionVersion": 2,
        "systemVersion": 1,
        "stateVersion": 1
    },
    "id": "getblock.io"
}

```

## Response Parameters

| Field                     | Type    | Description                                        |
| ------------------------- | ------- | -------------------------------------------------- |
| result.specName           | string  | Name of the runtime spec                           |
| result.implName           | string  | Name of the runtime implementation                 |
| result.specVersion        | integer | Spec version — increments on runtime upgrades      |
| result.transactionVersion | integer | Transaction format version                         |
| result.apis               | array   | List of runtime API identifiers and their versions |

## Use Cases

* Detecting runtime upgrades
* Selecting the correct extrinsic format for signing
* Capability detection in client libraries

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
const result = await api.rpc.chain.getRuntimeVersion();
console.log(result.toJSON());

// Or call the raw JSON-RPC method:
const raw = await api.rpc.rpc.methods();
// const raw = await (api as any)._rpcCore.provider.send('chain_getRuntimeVersion', []);
```
{% endtab %}

{% tab title="substrate-interface (Python)" %}
```python
from substrateinterface import SubstrateInterface

substrate = SubstrateInterface(url='wss://go.getblock.io/<ACCESS-TOKEN>/')

# Generic RPC call:
result = substrate.rpc_request('chain_getRuntimeVersion', [])
print(result)
```
{% endtab %}
{% endtabs %}
