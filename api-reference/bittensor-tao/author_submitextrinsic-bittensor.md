# author\_submitextrinsic bittensor

{% hint style="info" %}
**Substrate native JSON-RPC method.** Call against a GetBlock endpoint configured for the Substrate interface. For typed SDK access, use [Polkadot.js](https://polkadot.js.org/docs/api) (TypeScript) or [substrateinterface](https://github.com/polkascan/py-substrate-interface) (Python).
{% endhint %}

Submits a SCALE-encoded, signed extrinsic to the network. Returns the extrinsic hash. Does NOT wait for inclusion — use `author_submitAndWatchExtrinsic` (WSS) if you need inclusion status.

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
        "0x4502840022df56fa4a52f1eb96d4f1a9cdfb4cfbe5e7eb4..."
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
        "0x4502840022df56fa4a52f1eb96d4f1a9cdfb4cfbe5e7eb4..."
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
        "0x4502840022df56fa4a52f1eb96d4f1a9cdfb4cfbe5e7eb4..."
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
                "0x4502840022df56fa4a52f1eb96d4f1a9cdfb4cfbe5e7eb4..."
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
    "result": "0x8f7a2e3d4b5c6f1a9e8d7c6b5a4f3e2d1c0b9a8e7f6d5c4b3a2e1d0c9b8a7f6e"
}
```
{% endcode %}

## Response Parameters

| Field  | Type   | Description                                                               |
| ------ | ------ | ------------------------------------------------------------------------- |
| result | string | Extrinsic hash — use this to look up inclusion via subsequent block scans |

## Use Cases

* Submitting TAO transfers, staking calls, or subnet registrations
* Server-side wallet relayers that broadcast pre-signed extrinsics
* Replaying offline-signed extrinsics from cold storage

## Error Handling

| Status Code | Error Message       | Cause                                                                                                                      |
| ----------- | ------------------- | -------------------------------------------------------------------------------------------------------------------------- |
| 403         | Forbidden           | Missing or invalid `<ACCESS-TOKEN>`                                                                                        |
| -32602      | Invalid params      | Request parameters are missing or malformed                                                                                |
| -32601      | Method not found    | Method does not exist on this interface — check whether you're calling a Substrate method on an EVM endpoint or vice versa |
| -32603      | Internal error      | Server-side error while processing the request                                                                             |
| 429         | Too Many Requests   | Rate limit exceeded for your plan                                                                                          |
| 1010        | Invalid Transaction | The extrinsic is invalid (bad signature, nonce, or runtime version)                                                        |
| 1011        | Stale               | Nonce is too low — the account has already used this nonce                                                                 |
| 1012        | Future              | Nonce is too high — there's a gap in the nonce sequence                                                                    |
| 1013        | Exhausts Resources  | Extrinsic would exhaust block resources                                                                                    |

## SDK Integration

{% tabs %}
{% tab title="Polkadot.js (TypeScript)" %}
```javascript
import { ApiPromise, WsProvider } from '@polkadot/api';

const provider = new WsProvider('wss://go.getblock.io/<ACCESS-TOKEN>/');
const api = await ApiPromise.create({ provider });

// Typed wrapper: api.rpc.author.submitExtrinsic(...)
const result = await api.rpc.author.submitExtrinsic("0x4502840022df56fa4a52f1eb96d4f1a9cdfb4cfbe5e7eb4...");
console.log(result.toHuman());

// Or use the raw RPC interface for any method:
// const raw = await api.rpc.send({ method: 'author_submitExtrinsic', params: ["0x4502840022df56fa4a52f1eb96d4f1a9cdfb4cfbe5e7eb4..."] });
```
{% endtab %}

{% tab title="substrateinterface (Python)" %}
```python
from substrateinterface import SubstrateInterface

substrate = SubstrateInterface(url="wss://go.getblock.io/<ACCESS-TOKEN>/")

# Generic RPC call:
result = substrate.rpc_request("author_submitExtrinsic", ["0x4502840022df56fa4a52f1eb96d4f1a9cdfb4cfbe5e7eb4..."])
print(result)
```
{% endtab %}
{% endtabs %}
