# system\_chaintype bittensor

{% hint style="info" %}
**Substrate native JSON-RPC method.** Call against a GetBlock endpoint configured for the Substrate interface. For typed SDK access, use [Polkadot.js](https://polkadot.js.org/docs/api) (TypeScript) or [substrateinterface](https://github.com/polkascan/py-substrate-interface) (Python).
{% endhint %}

Returns the type of the chain — `Live` for production networks, `Development` or `Local` for development chains.

## Parameters

* None

## Request Example

{% tabs %}
{% tab title="cURL" %}
```bash
curl --location --request POST 'https://go.getblock.io/<ACCESS-TOKEN>/' \
--header 'Content-Type: application/json' \
--data-raw '{
    "jsonrpc": "2.0",
    "method": "system_chainType",
    "params": [],
    "id": "getblock.io"
}'
```
{% endtab %}

{% tab title="JavaScript (Axios)" %}
```javascript
import axios from 'axios';

const data = JSON.stringify({
    "jsonrpc": "2.0",
    "method": "system_chainType",
    "params": [],
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
    "method": "system_chainType",
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
        "method": "system_chainType",
        "params": [],
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
    "result": "Live"
}
```
{% endcode %}

## Response Parameters

| Field  | Type   | Description                                                  |
| ------ | ------ | ------------------------------------------------------------ |
| result | string | Chain type — one of `Live`, `Development`, `Local`, `Custom` |

## Use Cases

* Detecting whether an endpoint is a production network or development chain
* Gating destructive operations to non-production environments

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

// Typed wrapper: api.rpc.system.chainType(...)
const result = await api.rpc.system.chainType();
console.log(result.toHuman());

// Or use the raw RPC interface for any method:
// const raw = await api.rpc.send({ method: 'system_chainType', params: [] });
```
{% endtab %}

{% tab title="substrateinterface (Python)" %}
```python
from substrateinterface import SubstrateInterface

substrate = SubstrateInterface(url="wss://go.getblock.io/<ACCESS-TOKEN>/")

# Generic RPC call:
result = substrate.rpc_request("system_chainType", [])
print(result)
```
{% endtab %}
{% endtabs %}
