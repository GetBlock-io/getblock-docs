# payment\_queryinfo bittensor

{% hint style="info" %}
**Substrate native JSON-RPC method.** Call against a GetBlock endpoint configured for the Substrate interface. For typed SDK access, use [Polkadot.js](https://polkadot.js.org/docs/api) (TypeScript) or [substrateinterface](https://github.com/polkascan/py-substrate-interface) (Python).
{% endhint %}

Returns fee, weight, and class information for a given extrinsic — without submitting it. Used to estimate transaction costs before signing.

## Parameters

| Parameter | Type   | Required | Description                                                       |
| --------- | ------ | -------- | ----------------------------------------------------------------- |
| extrinsic | string | Yes      | Hex-encoded SCALE-serialized extrinsic (may or may not be signed) |
| blockHash | string | No       | Block hash to query at. Omit for the latest block.                |

## Request Example

{% tabs %}
{% tab title="cURL" %}
```bash
curl --location --request POST 'https://go.getblock.io/<ACCESS-TOKEN>/' \
--header 'Content-Type: application/json' \
--data-raw '{
    "jsonrpc": "2.0",
    "method": "payment_queryInfo",
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
    "method": "payment_queryInfo",
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
    "method": "payment_queryInfo",
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
        "method": "payment_queryInfo",
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
    "result": {
        "weight": {
            "refTime": 195955000,
            "proofSize": 3593
        },
        "class": "Normal",
        "partialFee": "10406640"
    }
}
```
{% endcode %}

## Response Parameters

| Field                   | Type    | Description                                                    |
| ----------------------- | ------- | -------------------------------------------------------------- |
| result.weight.refTime   | integer | Computation cost (reference time, picoseconds)                 |
| result.weight.proofSize | integer | PoV (proof of validity) size in bytes                          |
| result.class            | string  | Dispatch class — `Normal`, `Operational`, or `Mandatory`       |
| result.partialFee       | string  | Estimated fee in rao (1 TAO = 10^9 rao) — does not include tip |

## Use Cases

* Pre-flight fee estimation for wallet UIs
* Detecting excessively expensive extrinsics before submission
* Building fee oracles for TAO/USD denominated estimates

## Error Handling

| Status Code | Error Message     | Cause                                                                                                                      |
| ----------- | ----------------- | -------------------------------------------------------------------------------------------------------------------------- |
| 403         | Forbidden         | Missing or invalid `<ACCESS-TOKEN>`                                                                                        |
| -32602      | Invalid params    | Request parameters are missing or malformed                                                                                |
| -32601      | Method not found  | Method does not exist on this interface — check whether you're calling a Substrate method on an EVM endpoint or vice versa |
| -32603      | Internal error    | Server-side error while processing the request                                                                             |
| 429         | Too Many Requests | Rate limit exceeded for your plan                                                                                          |

## SDK Integration

{% tabs %}
{% tab title="Polkadot.js (TypeScript)" %}
```javascript
import { ApiPromise, WsProvider } from '@polkadot/api';

const provider = new WsProvider('wss://go.getblock.io/<ACCESS-TOKEN>/');
const api = await ApiPromise.create({ provider });

// Typed wrapper: api.rpc.payment.queryInfo(...)
const result = await api.rpc.payment.queryInfo("0x4502840022df56fa4a52f1eb96d4f1a9cdfb4cfbe5e7eb4...");
console.log(result.toHuman());

// Or use the raw RPC interface for any method:
// const raw = await api.rpc.send({ method: 'payment_queryInfo', params: ["0x4502840022df56fa4a52f1eb96d4f1a9cdfb4cfbe5e7eb4..."] });
```
{% endtab %}

{% tab title="substrateinterface (Python)" %}
```python
from substrateinterface import SubstrateInterface

substrate = SubstrateInterface(url="wss://go.getblock.io/<ACCESS-TOKEN>/")

# Generic RPC call:
result = substrate.rpc_request("payment_queryInfo", ["0x4502840022df56fa4a52f1eb96d4f1a9cdfb4cfbe5e7eb4..."])
print(result)
```
{% endtab %}
{% endtabs %}
