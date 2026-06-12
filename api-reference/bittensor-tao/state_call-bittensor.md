# state\_call bittensor

{% hint style="info" %}
**Substrate native JSON-RPC method.** Call against a GetBlock endpoint configured for the Substrate interface. For typed SDK access, use [Polkadot.js](https://polkadot.js.org/docs/api) (TypeScript) or [substrateinterface](https://github.com/polkascan/py-substrate-interface) (Python).
{% endhint %}

Executes a runtime API call — the bridge for accessing typed runtime APIs that aren't exposed as direct RPCs. This is how Polkadot.js routes calls like `api.call.subnetInfoRuntimeApi.getSubnetInfo(netuid)` under the hood. For Bittensor, the direct `subnetInfo_*` / `neuronInfo_*` / `delegateInfo_*` RPCs are often easier to use.

## Parameters

| Parameter | Type   | Required | Description                                                                                         |
| --------- | ------ | -------- | --------------------------------------------------------------------------------------------------- |
| method    | string | Yes      | Runtime API method — `<RuntimeApiName>_<methodName>` (e.g. `SubnetInfoRuntimeApi_get_subnets_info`) |
| data      | string | Yes      | SCALE-encoded call data (hex)                                                                       |
| blockHash | string | No       | Block hash to execute at                                                                            |

## Request Example

{% tabs %}
{% tab title="cURL" %}
```bash
curl --location --request POST 'https://go.getblock.io/<ACCESS-TOKEN>/' \
--header 'Content-Type: application/json' \
--data-raw '{
    "jsonrpc": "2.0",
    "method": "state_call",
    "params": [
        "SubnetInfoRuntimeApi_get_subnets_info",
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
    "method": "state_call",
    "params": [
        "SubnetInfoRuntimeApi_get_subnets_info",
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
    "method": "state_call",
    "params": [
        "SubnetInfoRuntimeApi_get_subnets_info",
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
        "method": "state_call",
        "params": [
                "SubnetInfoRuntimeApi_get_subnets_info",
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
    "result": "0x0100000010616c70686100040100000050a02f00..."
}
```
{% endcode %}

## Response Parameters

| Field  | Type   | Description                                              |
| ------ | ------ | -------------------------------------------------------- |
| result | string | Hex-encoded SCALE-encoded result of the runtime API call |

## Use Cases

* Calling runtime APIs that don't have direct RPC bindings
* Querying typed Bittensor runtime APIs (subnet info, neuron info) at the SCALE level
* Backwards compatibility — many things eventually move from custom RPC to runtime APIs

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

// Typed wrapper: api.rpc.state.call(...)
const result = await api.rpc.state.call("SubnetInfoRuntimeApi_get_subnets_info", "0x");
console.log(result.toHuman());

// Or use the raw RPC interface for any method:
// const raw = await api.rpc.send({ method: 'state_call', params: ["SubnetInfoRuntimeApi_get_subnets_info", "0x"] });
```
{% endtab %}

{% tab title="substrateinterface (Python)" %}
```python
from substrateinterface import SubstrateInterface

substrate = SubstrateInterface(url="wss://go.getblock.io/<ACCESS-TOKEN>/")

# Generic RPC call:
result = substrate.rpc_request("state_call", ["SubnetInfoRuntimeApi_get_subnets_info", "0x"])
print(result)
```
{% endtab %}
{% endtabs %}
