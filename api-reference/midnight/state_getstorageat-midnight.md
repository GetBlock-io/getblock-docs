---
description: >-
  Example code for the state_getStorageAt JSON-RPC method. Complete guide on how
  to use state_getStorageAt JSON-RPC in GetBlock Web3 documentation.
---

# state\_getStorageAt - Midnight

This method returns the value associated with a specific storage key in a given block. Functionally equivalent to `state_getStorage` with an explicit `hash`.

## Parameters

| Parameter | Type   | Required | Description                  |
| --------- | ------ | -------- | ---------------------------- |
| key       | string | Yes      | Hex-encoded storage key      |
| hash      | string | Yes      | Block hash at which to query |

## Request Example

{% tabs %}
{% tab title="cURL" %}
```bash
curl --location --request POST 'https://go.getblock.io/<ACCESS-TOKEN>/' \
--header 'Content-Type: application/json' \
--data-raw '{
    "jsonrpc": "2.0",
    "method": "state_getStorageAt",
    "params": [
        "0x26aa394eea5630e07c48ae0c9558cef7b99d880ec681799c0cf30e8886371da9",
        "0xed250c4a644b193c45a4e2101c616962dc8431e68c04798c4808b7283c195825"
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
    "method": "state_getStorageAt",
    "params": [
        "0x26aa394eea5630e07c48ae0c9558cef7b99d880ec681799c0cf30e8886371da9",
        "0xed250c4a644b193c45a4e2101c616962dc8431e68c04798c4808b7283c195825"
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
    "method": "state_getStorageAt",
    "params": [
        "0x26aa394eea5630e07c48ae0c9558cef7b99d880ec681799c0cf30e8886371da9",
        "0xed250c4a644b193c45a4e2101c616962dc8431e68c04798c4808b7283c195825"
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
        "method": "state_getStorageAt",
        "params": [
                "0x26aa394eea5630e07c48ae0c9558cef7b99d880ec681799c0cf30e8886371da9",
                "0xed250c4a644b193c45a4e2101c616962dc8431e68c04798c4808b7283c195825"
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
    "result": "0x040000000000000000"
}
```
{% endcode %}

## Response Parameters

| Field  | Type           | Description                                                      |
| ------ | -------------- | ---------------------------------------------------------------- |
| result | string \| null | Hex-encoded storage value at the given block, or `null` if unset |

## Use Cases

* Historical balance and state queries
* Reconstructing the past state for audits
* Forensics and analytics

## Error Handling

| Status Code | Error Message     | Cause                                              |
| ----------- | ----------------- | -------------------------------------------------- |
| 403         | Forbidden         | Missing or invalid `<ACCESS-TOKEN>`                |
| -32602      | Invalid params    | Request parameters are missing or malformed        |
| 429         | Too Many Requests | Rate limit exceeded for your plan                  |
| 4003        | Client error      | UnknownBlock: Header was not found in the database |

## SDK Integration

{% tabs %}
{% tab title="@polkadot/api (JavaScript)" %}
{% code overflow="wrap" %}
```javascript
import { ApiPromise, WsProvider } from '@polkadot/api';

const wsProvider = new WsProvider('wss://go.getblock.io/<ACCESS-TOKEN>/');
const api = await ApiPromise.create({ provider: wsProvider });

// Call via the typed API:
const result = await api.rpc.state.getStorageAt("0x26aa394eea5630e07c48ae0c9558cef7b99d880ec681799c0cf30e8886371da9", "0x87f5c08ba8334e4ee1642ff9646e94dd46080b71fff03714be8b36f22e481b95");
console.log(result.toJSON());

// Or call the raw JSON-RPC method:
const raw = await api.rpc.rpc.methods();
// const raw = await (api as any)._rpcCore.provider.send('state_getStorageAt', ["0x26aa394eea5630e07c48ae0c9558cef7b99d880ec681799c0cf30e8886371da9", "0x87f5c08ba8334e4ee1642ff9646e94dd46080b71fff03714be8b36f22e481b95"]);
```
{% endcode %}
{% endtab %}

{% tab title="substrate-interface (Python)" %}
```python
from substrateinterface import SubstrateInterface

substrate = SubstrateInterface(url='wss://go.getblock.io/<ACCESS-TOKEN>/')

# Generic RPC call:
result = substrate.rpc_request('state_getStorageAt', ["0x26aa394eea5630e07c48ae0c9558cef7b99d880ec681799c0cf30e8886371da9", "0x87f5c08ba8334e4ee1642ff9646e94dd46080b71fff03714be8b36f22e481b95"])
print(result)
```
{% endtab %}
{% endtabs %}
