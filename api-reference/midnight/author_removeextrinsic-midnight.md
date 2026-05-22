---
description: >-
  Example code for the author_removeExtrinsic JSON-RPC method. Complete guide on
  how to use author_removeExtrinsic JSON-RPC in GetBlock Web3 documentation.
---

# author\_removeExtrinsic - Midnight

This method removes one or more extrinsics from the transaction pool. Most managed RPC providers disable this method since it affects shared node state.

## Parameters

| Parameter   | Type            | Required | Description                                                               |
| ----------- | --------------- | -------- | ------------------------------------------------------------------------- |
| bytesOrHash | array of object | Yes      | Array of either `{Hash: '0x…'}` or `{Extrinsic: '0x…'}` entries to remove |

## Request Example

{% tabs %}
{% tab title="cURL" %}
```bash
curl --location --request POST 'https://go.getblock.io/<ACCESS-TOKEN>/' \
--header 'Content-Type: application/json' \
--data-raw '{
    "jsonrpc": "2.0",
    "method": "author_removeExtrinsic",
    "params": [
        [
            {
                "Hash": "0x88df016429689c079f3b2f6ad39fa052532c56795b733da78a91ebe6a713944b"
            }
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
    "method": "author_removeExtrinsic",
    "params": [
        [
            {
                "Hash": "0x88df016429689c079f3b2f6ad39fa052532c56795b733da78a91ebe6a713944b"
            }
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
    "method": "author_removeExtrinsic",
    "params": [
        [
            {
                "Hash": "0x88df016429689c079f3b2f6ad39fa052532c56795b733da78a91ebe6a713944b"
            }
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
        "method": "author_removeExtrinsic",
        "params": [
                [
                        {
                                "Hash": "0x88df016429689c079f3b2f6ad39fa052532c56795b733da78a91ebe6a713944b"
                        }
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
    "id": "getblock.io",
    "result": [
        "0x88df016429689c079f3b2f6ad39fa052532c56795b733da78a91ebe6a713944b"
    ]
}
```

## Response Parameters

| Field  | Type            | Description                                             |
| ------ | --------------- | ------------------------------------------------------- |
| result | array of string | Hashes of the extrinsics that were successfully removed |

## Use Cases

* Cleaning up replaced or invalid extrinsics in a local dev node
* Test harnesses that need to reset pool state

## Error Handling

| Status Code | Error Message     | Cause                                                                                    |
| ----------- | ----------------- | ---------------------------------------------------------------------------------------- |
| 403         | Forbidden         | Missing or invalid `<ACCESS-TOKEN>`                                                      |
| -32602      | Invalid params    | Request parameters are missing or malformed                                              |
| 429         | Too Many Requests | Rate limit exceeded for your plan                                                        |
| -32601      | Method not found  | Disabled by most managed RPC providers; available only on dedicated or self-hosted nodes |

## SDK Integration

{% tabs %}
{% tab title="@polkadot/api (JavaScript)" %}
```javascript
import { ApiPromise, WsProvider } from '@polkadot/api';

const wsProvider = new WsProvider('wss://go.getblock.io/<ACCESS-TOKEN>/');
const api = await ApiPromise.create({ provider: wsProvider });

// Call via the typed API:
const result = await api.rpc.author.removeExtrinsic([{"Hash": "0x88df016429689c079f3b2f6ad39fa052532c56795b733da78a91ebe6a713944b"}]);
console.log(result.toJSON());

// Or call the raw JSON-RPC method:
const raw = await api.rpc.rpc.methods();
// const raw = await (api as any)._rpcCore.provider.send('author_removeExtrinsic', [[{"Hash": "0x88df016429689c079f3b2f6ad39fa052532c56795b733da78a91ebe6a713944b"}]]);
```
{% endtab %}

{% tab title="substrate-interface (Python)" %}
```python
from substrateinterface import SubstrateInterface

substrate = SubstrateInterface(url='wss://go.getblock.io/<ACCESS-TOKEN>/')

# Generic RPC call:
result = substrate.rpc_request('author_removeExtrinsic', [[{"Hash": "0x88df016429689c079f3b2f6ad39fa052532c56795b733da78a91ebe6a713944b"}]])
print(result)
```
{% endtab %}
{% endtabs %}
