# state\_getstorage bittensor

{% hint style="info" %}
**Substrate native JSON-RPC method.** Call against a GetBlock endpoint configured for the Substrate interface. For typed SDK access, use [Polkadot.js](https://polkadot.js.org/docs/api) (TypeScript) or [substrateinterface](https://github.com/polkascan/py-substrate-interface) (Python).
{% endhint %}

Returns the raw storage value at a given storage key. Storage keys are constructed by hashing the pallet name + item name + (optionally) the parameter — typically computed by SDKs like Polkadot.js rather than constructed by hand.

## Parameters

| Parameter | Type   | Required | Description                                                                                  |
| --------- | ------ | -------- | -------------------------------------------------------------------------------------------- |
| key       | string | Yes      | Hex-encoded storage key (typically computed via `xxhash128(pallet) + xxhash128(item) + ...`) |
| blockHash | string | No       | Block hash to query at. Omit for the latest block.                                           |

## Request Example

{% tabs %}
{% tab title="cURL" %}
```bash
curl --location --request POST 'https://go.getblock.io/<ACCESS-TOKEN>/' \
--header 'Content-Type: application/json' \
--data-raw '{
    "jsonrpc": "2.0",
    "method": "state_getStorage",
    "params": [
        "0x26aa394eea5630e07c48ae0c9558cef7b99d880ec681799c0cf30e8886371da9"
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
    "method": "state_getStorage",
    "params": [
        "0x26aa394eea5630e07c48ae0c9558cef7b99d880ec681799c0cf30e8886371da9"
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
    "method": "state_getStorage",
    "params": [
        "0x26aa394eea5630e07c48ae0c9558cef7b99d880ec681799c0cf30e8886371da9"
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
        "method": "state_getStorage",
        "params": [
                "0x26aa394eea5630e07c48ae0c9558cef7b99d880ec681799c0cf30e8886371da9"
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
    "result": "0x0000000000000000010000000000000000000000000000000000000000000000..."
}
```
{% endcode %}

## Response Parameters

| Field  | Type           | Description                                                               |
| ------ | -------------- | ------------------------------------------------------------------------- |
| result | string \| null | Hex-encoded SCALE-encoded value, or `null` if the storage item is not set |

## Use Cases

* Reading raw chain state when you know the exact storage layout
* Building custom indexers that decode SCALE manually
* Querying storage at a specific historical block

## Error Handling

| Status Code | Error Message       | Cause                                                                                                                      |
| ----------- | ------------------- | -------------------------------------------------------------------------------------------------------------------------- |
| 403         | Forbidden           | Missing or invalid `<ACCESS-TOKEN>`                                                                                        |
| -32602      | Invalid params      | Request parameters are missing or malformed                                                                                |
| -32601      | Method not found    | Method does not exist on this interface — check whether you're calling a Substrate method on an EVM endpoint or vice versa |
| -32603      | Internal error      | Server-side error while processing the request                                                                             |
| 429         | Too Many Requests   | Rate limit exceeded for your plan                                                                                          |
| -32602      | Invalid storage key | Storage key must be valid hex starting with `0x`                                                                           |

## SDK Integration

{% tabs %}
{% tab title="Polkadot.js (TypeScript)" %}
```javascript
import { ApiPromise, WsProvider } from '@polkadot/api';

const provider = new WsProvider('wss://go.getblock.io/<ACCESS-TOKEN>/');
const api = await ApiPromise.create({ provider });

// Typed wrapper: api.rpc.state.getStorage(...)
const result = await api.rpc.state.getStorage("0x26aa394eea5630e07c48ae0c9558cef7b99d880ec681799c0cf30e8886371da9");
console.log(result.toHuman());

// Or use the raw RPC interface for any method:
// const raw = await api.rpc.send({ method: 'state_getStorage', params: ["0x26aa394eea5630e07c48ae0c9558cef7b99d880ec681799c0cf30e8886371da9"] });
```
{% endtab %}

{% tab title="substrateinterface (Python)" %}
```python
from substrateinterface import SubstrateInterface

substrate = SubstrateInterface(url="wss://go.getblock.io/<ACCESS-TOKEN>/")

# Generic RPC call:
result = substrate.rpc_request("state_getStorage", ["0x26aa394eea5630e07c48ae0c9558cef7b99d880ec681799c0cf30e8886371da9"])
print(result)
```
{% endtab %}
{% endtabs %}
