---
description: >-
  Example code for the childstate_getStorageEntries JSON-RPC method. Complete
  guide on how to use childstate_getStorageEntries JSON-RPC in GetBlock Web3
  documentation.
---

# childstate\_getStorageEntries - Midnight

This method returns multiple values from a child trie in a single call. More efficient than calling `childstate_getStorage` once per key.

## Parameters

| Parameter       | Type            | Required | Description                                                |
| --------------- | --------------- | -------- | ---------------------------------------------------------- |
| childStorageKey | string          | Yes      | Hex-encoded identifier of the child storage trie           |
| keys            | array of string | Yes      | Array of hex-encoded keys to read                          |
| hash            | string          | No       | Block hash to query at. Defaults to the current best block |

## Request Example

{% tabs %}
{% tab title="cURL" %}
```bash
curl --location --request POST 'https://go.getblock.io/<ACCESS-TOKEN>/' \
--header 'Content-Type: application/json' \
--data-raw '{
    "jsonrpc": "2.0",
    "method": "childstate_getStorageEntries",
    "params": [
        "0x3a6368696c645f73746f726167653a64656661756c743a",
        [
            "0x80",
            "0x81"
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
    "method": "childstate_getStorageEntries",
    "params": [
        "0x3a6368696c645f73746f726167653a64656661756c743a",
        [
            "0x80",
            "0x81"
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
    "method": "childstate_getStorageEntries",
    "params": [
        "0x3a6368696c645f73746f726167653a64656661756c743a",
        [
            "0x80",
            "0x81"
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
        "method": "childstate_getStorageEntries",
        "params": [
                "0x3a6368696c645f73746f726167653a64656661756c743a",
                [
                        "0x80",
                        "0x81"
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

{% code overflow="wrap" %}
```json
{
    "jsonrpc": "2.0",
    "id": "getblock.io",
    "result": [
        "0x040000000000000000",
        "0x080000000000000000"
    ]
}
```
{% endcode %}

## Response Parameters

| Field  | Type                    | Description                                                          |
| ------ | ----------------------- | -------------------------------------------------------------------- |
| result | array of string \| null | Array of hex-encoded values, in the same order as the requested keys |

## Use Cases

* Batched reads of contract state
* Reducing round-trips when fetching multiple related values

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
const result = await api.rpc.childstate.getStorageEntries("0x3a6368696c645f73746f726167653a64656661756c743a", ["0x80", "0x81"]);
console.log(result.toJSON());

// Or call the raw JSON-RPC method:
const raw = await api.rpc.rpc.methods();
// const raw = await (api as any)._rpcCore.provider.send('childstate_getStorageEntries', ["0x3a6368696c645f73746f726167653a64656661756c743a", ["0x80", "0x81"]]);
```
{% endtab %}

{% tab title="substrate-interface (Python)" %}
```python
from substrateinterface import SubstrateInterface

substrate = SubstrateInterface(url='wss://go.getblock.io/<ACCESS-TOKEN>/')

# Generic RPC call:
result = substrate.rpc_request('childstate_getStorageEntries', ["0x3a6368696c645f73746f726167653a64656661756c743a", ["0x80", "0x81"]])
print(result)
```
{% endtab %}
{% endtabs %}
