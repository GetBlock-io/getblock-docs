# chain\_getblock bittensor

{% hint style="info" %}
**Substrate native JSON-RPC method.** Call against a GetBlock endpoint configured for the Substrate interface. For typed SDK access, use [Polkadot.js](https://polkadot.js.org/docs/api) (TypeScript) or [substrateinterface](https://github.com/polkascan/py-substrate-interface) (Python).
{% endhint %}

Returns the full block (header + body of extrinsics) for a given block hash. Pass `null` (or omit) to get the latest block.

## Parameters

| Parameter | Type   | Required | Description                                        |
| --------- | ------ | -------- | -------------------------------------------------- |
| blockHash | string | No       | Hex-encoded block hash. Omit for the latest block. |

## Request Example

{% tabs %}
{% tab title="cURL" %}
```bash
curl --location --request POST 'https://go.getblock.io/<ACCESS-TOKEN>/' \
--header 'Content-Type: application/json' \
--data-raw '{
    "jsonrpc": "2.0",
    "method": "chain_getBlock",
    "params": [
        "0xc0a6cfca5d4f9e1cb8a5c4b3d2e7a9c4f1b8e6d5c3a2b1f9e8d7c6b5a4f3e2d1c"
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
    "method": "chain_getBlock",
    "params": [
        "0xc0a6cfca5d4f9e1cb8a5c4b3d2e7a9c4f1b8e6d5c3a2b1f9e8d7c6b5a4f3e2d1c"
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
    "method": "chain_getBlock",
    "params": [
        "0xc0a6cfca5d4f9e1cb8a5c4b3d2e7a9c4f1b8e6d5c3a2b1f9e8d7c6b5a4f3e2d1c"
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
        "method": "chain_getBlock",
        "params": [
                "0xc0a6cfca5d4f9e1cb8a5c4b3d2e7a9c4f1b8e6d5c3a2b1f9e8d7c6b5a4f3e2d1c"
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
    "result": {
        "block": {
            "header": {
                "parentHash": "0x47edbdc8da5cd1b3127ea8f1ce5da6739fa39e7e6d2eb1c9b50f1c83de0a98b4",
                "number": "0x599f5a",
                "stateRoot": "0x123456789abcdef0123456789abcdef0123456789abcdef0123456789abcdef0",
                "extrinsicsRoot": "0xfedcba9876543210fedcba9876543210fedcba9876543210fedcba9876543210",
                "digest": {
                    "logs": []
                }
            },
            "extrinsics": [
                "0x280403000bb0eafd9b8401",
                "0x0400000000010000005847..."
            ]
        },
        "justifications": null
    }
}
```

## Response Parameters

| Field                      | Type            | Description                                                          |
| -------------------------- | --------------- | -------------------------------------------------------------------- |
| result.block.header        | object          | Block header — parentHash, number, stateRoot, extrinsicsRoot, digest |
| result.block.header.number | string          | Block number in hex                                                  |
| result.block.extrinsics    | array of string | SCALE-encoded extrinsics (hex)                                       |
| result.justifications      | object \| null  | Block finality justifications (typically null for HTTP queries)      |

## Use Cases

* Block-by-block indexing in explorers and analytics
* Replay analysis of historical blocks
* Inspecting extrinsics submitted in a specific block

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

// Typed wrapper: api.rpc.chain.getBlock(...)
const result = await api.rpc.chain.getBlock("0xc0a6cfca5d4f9e1cb8a5c4b3d2e7a9c4f1b8e6d5c3a2b1f9e8d7c6b5a4f3e2d1c");
console.log(result.toHuman());

// Or use the raw RPC interface for any method:
// const raw = await api.rpc.send({ method: 'chain_getBlock', params: ["0xc0a6cfca5d4f9e1cb8a5c4b3d2e7a9c4f1b8e6d5c3a2b1f9e8d7c6b5a4f3e2d1c"] });
```
{% endtab %}

{% tab title="substrateinterface (Python)" %}
```python
from substrateinterface import SubstrateInterface

substrate = SubstrateInterface(url="wss://go.getblock.io/<ACCESS-TOKEN>/")

# Generic RPC call:
result = substrate.rpc_request("chain_getBlock", ["0xc0a6cfca5d4f9e1cb8a5c4b3d2e7a9c4f1b8e6d5c3a2b1f9e8d7c6b5a4f3e2d1c"])
print(result)
```
{% endtab %}
{% endtabs %}
