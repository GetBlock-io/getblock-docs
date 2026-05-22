---
description: >-
  Example code for the author_submitExtrinsic JSON-RPC method. Complete guide on
  how to use author_submitExtrinsic JSON-RPC in GetBlock Web3 documentation.
---

# author\_submitExtrinsic - Midnight

This method submits a signed extrinsic to the network for inclusion in a future block. The extrinsic must be SCALE-encoded and pre-signed; this is the Substrate equivalent of `eth_sendRawTransaction`. Returns the extrinsic hash for tracking.

## Parameters

| Parameter | Type   | Required | Description                                   |
| --------- | ------ | -------- | --------------------------------------------- |
| extrinsic | string | Yes      | Hex-encoded SCALE-serialized signed extrinsic |

## Request Example

{% tabs %}
{% tab title="cURL" %}
```bash
curl --location --request POST 'https://go.getblock.io/<ACCESS-TOKEN>/' \
--header 'Content-Type: application/json' \
--data-raw '{
    "jsonrpc": "2.0",
    "method": "author_submitExtrinsic",
    "params": [
        "0x280402000bf83d6e4d9101\u2026"
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
    "method": "author_submitExtrinsic",
    "params": [
        "0x280402000bf83d6e4d9101\u2026"
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
    "method": "author_submitExtrinsic",
    "params": [
        "0x280402000bf83d6e4d9101\u2026"
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
        "method": "author_submitExtrinsic",
        "params": [
                "0x280402000bf83d6e4d9101\u2026"
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
    "result": "0x88df016429689c079f3b2f6ad39fa052532c56795b733da78a91ebe6a713944b"
}
```
{% endcode %}

## Response Parameters

| Field  | Type   | Description                                                  |
| ------ | ------ | ------------------------------------------------------------ |
| result | string | Hash of the submitted extrinsic, used for tracking inclusion |

## Use Cases

* Submitting NIGHT or DUST transfers
* Broadcasting smart contract calls
* Deploying Compact contracts
* Any state-changing interaction with Midnight

## Error Handling

| Status Code | Error Message       | Cause                                                               |
| ----------- | ------------------- | ------------------------------------------------------------------- |
| 404         |                     | Missing or invalid `<ACCESS-TOKEN>`                                 |
| -32602      | Invalid params      | Request parameters are missing or malformed                         |
| -32053      | Method not found    | API key is not allowed to access method                             |
| 429         | Too Many Requests   | Rate limit exceeded for your plan                                   |
| 1010        | Invalid Transaction | Signature invalid, nonce mismatch, or extrinsic malformed           |
| 1011        | Pool error          | Transaction pool rejected the extrinsic (full, banned, or replaced) |

## SDK Integration

{% tabs %}
{% tab title="@polkadot/api (JavaScript)" %}
{% code overflow="wrap" %}
```javascript
import { ApiPromise, WsProvider } from '@polkadot/api';

const wsProvider = new WsProvider('wss://go.getblock.io/<ACCESS-TOKEN>/');
const api = await ApiPromise.create({ provider: wsProvider });

// Call via the typed API:
const result = await api.rpc.author.submitExtrinsic("0x280402000bf83d6e4d9101\u2026");
console.log(result.toJSON());

// Or call the raw JSON-RPC method:
const raw = await api.rpc.rpc.methods();
// const raw = await (api as any)._rpcCore.provider.send('author_submitExtrinsic', ["0x280402000bf83d6e4d9101\u2026"]);
```
{% endcode %}
{% endtab %}

{% tab title="substrate-interface (Python)" %}
```python
from substrateinterface import SubstrateInterface

substrate = SubstrateInterface(url='wss://go.getblock.io/<ACCESS-TOKEN>/')

# Generic RPC call:
result = substrate.rpc_request('author_submitExtrinsic', ["0x280402000bf83d6e4d9101\u2026"])
print(result)
```
{% endtab %}
{% endtabs %}
