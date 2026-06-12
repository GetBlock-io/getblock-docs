# rpc\_methods bittensor

{% hint style="info" %}
**Substrate native JSON-RPC method.** Call against a GetBlock endpoint configured for the Substrate interface. For typed SDK access, use [Polkadot.js](https://polkadot.js.org/docs/api) (TypeScript) or [substrateinterface](https://github.com/polkascan/py-substrate-interface) (Python).
{% endhint %}

Returns the list of all RPC methods exposed by the connected Subtensor node. Essential for API discovery, capability detection, and verifying that a node exposes the methods your application needs (including chain-specific custom methods like `subnetInfo_getSubnetsInfo`).

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
    "method": "rpc_methods",
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
    "method": "rpc_methods",
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
    "method": "rpc_methods",
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
        "method": "rpc_methods",
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
    "result": {
        "version": 1,
        "methods": [
            "author_pendingExtrinsics",
            "author_submitAndWatchExtrinsic",
            "author_submitExtrinsic",
            "chain_getBlock",
            "chain_getBlockHash",
            "chain_getFinalizedHead",
            "chain_getHeader",
            "chain_subscribeFinalizedHeads",
            "chain_subscribeNewHeads",
            "chain_unsubscribeAllHeads",
            "delegateInfo_getDelegate",
            "delegateInfo_getDelegated",
            "delegateInfo_getDelegates",
            "neuronInfo_getNeuron",
            "neuronInfo_getNeurons",
            "payment_queryFeeDetails",
            "payment_queryInfo",
            "rpc_methods",
            "state_call",
            "state_getMetadata",
            "state_getRuntimeVersion",
            "state_getStorage",
            "state_subscribeStorage",
            "subnetInfo_getLockCost",
            "subnetInfo_getSubnetHyperparams",
            "subnetInfo_getSubnetInfo",
            "subnetInfo_getSubnetsInfo",
            "system_accountNextIndex",
            "system_chain",
            "system_chainType",
            "system_health",
            "system_name",
            "system_properties",
            "system_version"
        ]
    }
}
```
{% endcode %}

## Response Parameters

| Field          | Type            | Description                                                  |
| -------------- | --------------- | ------------------------------------------------------------ |
| result.version | integer         | RPC version (always `1` currently)                           |
| result.methods | array of string | Alphabetically sorted list of all available RPC method names |

## Use Cases

* API capability discovery at application startup
* Verifying that a Subtensor node exposes the Bittensor-specific custom RPC namespaces (`subnetInfo_*`, `neuronInfo_*`, `delegateInfo_*`)
* Auto-generating client SDKs from the live method list
* Comparing method availability across different RPC providers

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

// Typed wrapper: api.rpc.rpc.methods(...)
const result = await api.rpc.rpc.methods();
console.log(result.toHuman());

// Or use the raw RPC interface for any method:
// const raw = await api.rpc.send({ method: 'rpc_methods', params: [] });
```
{% endtab %}

{% tab title="substrateinterface (Python)" %}
```python
from substrateinterface import SubstrateInterface

substrate = SubstrateInterface(url="wss://go.getblock.io/<ACCESS-TOKEN>/")

# Generic RPC call:
result = substrate.rpc_request("rpc_methods", [])
print(result)
```
{% endtab %}
{% endtabs %}
