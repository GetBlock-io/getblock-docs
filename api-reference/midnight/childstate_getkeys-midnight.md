---
description: >-
  Example code for the childstate_getKeys JSON-RPC method. Complete guide on how
  to use childstate_getKeys JSON-RPC in GetBlock Web3 documentation.
---

# childstate\_getKeys - Midnight

This method returns all storage keys in a child trie that match an optional prefix. Child tries are isolated key-value namespaces used by pallets such as `contracts` for per-contract storage.

## Parameters

| Parameter       | Type   | Required | Description                                                |
| --------------- | ------ | -------- | ---------------------------------------------------------- |
| childStorageKey | string | Yes      | Hex-encoded identifier of the child storage trie           |
| prefix          | string | Yes      | Hex-encoded key prefix within the child trie               |
| hash            | string | No       | Block hash to query at. Defaults to the current best block |

## Request Example

{% tabs %}
{% tab title="cURL" %}
```bash
curl --location --request POST 'https://go.getblock.io/<ACCESS-TOKEN>/' \
--header 'Content-Type: application/json' \
--data-raw '{
    "jsonrpc": "2.0",
    "method": "childstate_getKeys",
    "params": [
        "0x3a6368696c645f73746f726167653a64656661756c743a",
        "0x"
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
    "method": "childstate_getKeys",
    "params": [
        "0x3a6368696c645f73746f726167653a64656661756c743a",
        "0x"
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
    "method": "childstate_getKeys",
    "params": [
        "0x3a6368696c645f73746f726167653a64656661756c743a",
        "0x"
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
        "method": "childstate_getKeys",
        "params": [
                "0x3a6368696c645f73746f726167653a64656661756c743a",
                "0x"
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
        "0x80\u2026",
        "0x80\u2026"
    ]
}
```
{% endcode %}

## Response Parameters

| Field  | Type            | Description                                 |
| ------ | --------------- | ------------------------------------------- |
| result | array of string | Array of hex-encoded keys in the child trie |

## Use Cases

* Reading per-contract storage in WASM smart contracts
* Indexers that need to enumerate child trie contents
* Debugging contract state

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
const result = await api.rpc.childstate.getKeys("0x3a6368696c645f73746f726167653a64656661756c743a", "0x");
console.log(result.toJSON());

// Or call the raw JSON-RPC method:
const raw = await api.rpc.rpc.methods();
// const raw = await (api as any)._rpcCore.provider.send('childstate_getKeys', ["0x3a6368696c645f73746f726167653a64656661756c743a", "0x"]);
```
{% endtab %}

{% tab title="substrate-interface (Python)" %}
```python
from substrateinterface import SubstrateInterface

substrate = SubstrateInterface(url='wss://go.getblock.io/<ACCESS-TOKEN>/')

# Generic RPC call:
result = substrate.rpc_request('childstate_getKeys', ["0x3a6368696c645f73746f726167653a64656661756c743a", "0x"])
print(result)
```
{% endtab %}
{% endtabs %}
