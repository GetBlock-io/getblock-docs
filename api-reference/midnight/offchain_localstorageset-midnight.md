---
description: >-
  Example code for the offchain_localStorageSet JSON-RPC method. Complete guide
  on how to use offchain_localStorageSet JSON-RPC in GetBlock Web3
  documentation.
---

# offchain\_localStorageSet - Midnight

This method writes a value to the node's off-chain worker local storage. Note that managed RPC providers typically disable this method, since it can affect node behavior.

## Parameters

| Parameter | Type   | Required | Description                           |
| --------- | ------ | -------- | ------------------------------------- |
| kind      | string | Yes      | Storage kind: `PERSISTENT` or `LOCAL` |
| key       | string | Yes      | Hex-encoded storage key               |
| value     | string | Yes      | Hex-encoded value to store            |

## Request Example

{% tabs %}
{% tab title="cURL" %}
```bash
curl --location --request POST 'https://go.getblock.io/<ACCESS-TOKEN>/' \
--header 'Content-Type: application/json' \
--data-raw '{
    "jsonrpc": "2.0",
    "method": "offchain_localStorageSet",
    "params": [
        "PERSISTENT",
        "0x6d795f6b6579",
        "0x68656c6c6f"
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
    "method": "offchain_localStorageSet",
    "params": [
        "PERSISTENT",
        "0x6d795f6b6579",
        "0x68656c6c6f"
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
    "method": "offchain_localStorageSet",
    "params": [
        "PERSISTENT",
        "0x6d795f6b6579",
        "0x68656c6c6f"
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
        "method": "offchain_localStorageSet",
        "params": [
                "PERSISTENT",
                "0x6d795f6b6579",
                "0x68656c6c6f"
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
    "result": null
}
```
{% endcode %}

## Response Parameters

| Field  | Type | Description              |
| ------ | ---- | ------------------------ |
| result | null | Always `null` on success |

## Use Cases

* Seeding off-chain worker state during local development
* Test harnesses that need to pre-populate off-chain storage

## Error Handling

| Status Code | Error Message     | Cause                                       |
| ----------- | ----------------- | ------------------------------------------- |
| 404         | Not found         | Missing or invalid `<ACCESS-TOKEN>`         |
| -32602      | Invalid params    | Request parameters are missing or malformed |
| -32601      | Method not found  | RPC call is unsafe to be called externally  |
| 429         | Too Many Requests | Rate limit exceeded for your plan           |

## SDK Integration

{% tabs %}
{% tab title="@polkadot/api (JavaScript)" %}
```javascript
import { ApiPromise, WsProvider } from '@polkadot/api';

const wsProvider = new WsProvider('wss://go.getblock.io/<ACCESS-TOKEN>/');
const api = await ApiPromise.create({ provider: wsProvider });

// Call via the typed API:
const result = await api.rpc.offchain.localStorageSet("PERSISTENT", "0x6d795f6b6579", "0x68656c6c6f");
console.log(result.toJSON());

// Or call the raw JSON-RPC method:
const raw = await api.rpc.rpc.methods();
// const raw = await (api as any)._rpcCore.provider.send('offchain_localStorageSet', ["PERSISTENT", "0x6d795f6b6579", "0x68656c6c6f"]);
```
{% endtab %}

{% tab title="substrate-interface (Python)" %}
```python
from substrateinterface import SubstrateInterface

substrate = SubstrateInterface(url='wss://go.getblock.io/<ACCESS-TOKEN>/')

# Generic RPC call:
result = substrate.rpc_request('offchain_localStorageSet', ["PERSISTENT", "0x6d795f6b6579", "0x68656c6c6f"])
print(result)
```
{% endtab %}
{% endtabs %}
