---
description: >-
  Example code for the author_pendingExtrinsics JSON-RPC method. Complete guide
  on how to use author_pendingExtrinsics JSON-RPC in GetBlock Web3
  documentation.
---

# author\_pendingExtrinsics -Bittensor

Returns all extrinsics currently waiting in the transaction pool but not yet included in a block. The mempool view for Substrate.

{% hint style="info" %}
**Substrate native JSON-RPC method.** Call against a GetBlock endpoint configured for the Substrate interface. For typed SDK access, use [Polkadot.js](https://polkadot.js.org/docs/api) (TypeScript) or [substrateinterface](https://github.com/polkascan/py-substrate-interface) (Python).
{% endhint %}

## Parameters

* None

## Request Example

{% tabs %}
{% tab title="cURL" %}
{% code overflow="wrap" %}
```bash
curl --location --request POST 'https://shared.us-east-1.getblock.io/<Access-token>/' \
--header 'Content-Type: application/json' \
--data-raw '{
    "jsonrpc": "2.0",
    "method": "author_pendingExtrinsics",
    "params": [],
    "id": "getblock.io"
}'
```
{% endcode %}
{% endtab %}

{% tab title="JavaScript (Axios)" %}
```javascript
import axios from 'axios';

const data = JSON.stringify({
    "jsonrpc": "2.0",
    "method": "author_pendingExtrinsics",
    "params": [],
    "id": "getblock.io"
});

const config = {
    method: 'post',
    url: 'https://shared.us-east-1.getblock.io/<Access-token>/',
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

url = "https://shared.us-east-1.getblock.io/<Access-token>/"

payload = json.dumps({
    "jsonrpc": "2.0",
    "method": "author_pendingExtrinsics",
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
        "method": "author_pendingExtrinsics",
        "params": [],
        "id": "getblock.io"
});

    let response = client
        .post("https://shared.us-east-1.getblock.io/<Access-token>/")
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
"0x251104150002c4030000000000003710050000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000e40b5402000000000000000000000000000000000000000000000000000000407e0500000000000000000000000000000000000000000000000000000000000094c262216a8111d208433ad48fcdc05831d60afb0000000000000000000000000000000000000000000000000000000000000000110d5001f15e33c690fce560e595e4ffb5ac64ba559e34837486948e448c549476bd868125f4c482388586a426cc6489a49105c884bde681849ea49de5dd249af79d148574b1e5d4e59234cc94b345f534f885dc34a826e3c4ce488c84ea75dff4932596f4bd658934970499a0000532d0b1ba473258b8ec6bded2a5ed0d71ead59ecc94ad81b6d8a3c6c9866b51a70912d0cc4c4e08078266d2a947c78cb57b3466f31c805cbe39b329cfcb9fbe9"
    ]
}
```
{% endcode %}

## Response Parameters

| Field  | Type            | Description                                            |
| ------ | --------------- | ------------------------------------------------------ |
| result | array of string | Array of hex-encoded pending extrinsics in the mempool |

## Use Cases

* Mempool monitoring tools
* MEV / front-running detection on Subtensor
* Detecting pending extrinsics from specific accounts before inclusion

## Error Handling

| Status Code | Error Message     | Cause                                          |
| ----------- | ----------------- | ---------------------------------------------- |
| 403         | Forbidden         | Missing or invalid `<ACCESS-TOKEN>`            |
| -32602      | Invalid params    | Request parameters are missing or malformed    |
| -32603      | Internal error    | Server-side error while processing the request |
| 429         | Too Many Requests | Rate limit exceeded for your plan              |

## SDK Integration

{% tabs %}
{% tab title="Polkadot.js (TypeScript)" %}
{% code overflow="wrap" %}
```javascript
import { ApiPromise, WsProvider } from '@polkadot/api';

const provider = new WsProvider('wss://go.getblock.io/<ACCESS-TOKEN>/');
const api = await ApiPromise.create({ provider });

// Typed wrapper: api.rpc.author.pendingExtrinsics(...)
const result = await api.rpc.author.pendingExtrinsics();
console.log(result.toHuman());

// Or use the raw RPC interface for any method:
// const raw = await api.rpc.send({ method: 'author_pendingExtrinsics', params: [] });
```
{% endcode %}
{% endtab %}

{% tab title="substrateinterface (Python)" %}
```python
from substrateinterface import SubstrateInterface

substrate = SubstrateInterface(url="wss://go.getblock.io/<ACCESS-TOKEN>/")

# Generic RPC call:
result = substrate.rpc_request("author_pendingExtrinsics", [])
print(result)
```
{% endtab %}
{% endtabs %}
