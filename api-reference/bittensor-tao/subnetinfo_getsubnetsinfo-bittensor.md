# subnetinfo\_getsubnetsinfo bittensor

{% hint style="info" %}
**Bittensor-specific custom RPC method.** This method is implemented by the Subtensor runtime and is not part of the standard Substrate JSON-RPC surface. Call against a GetBlock endpoint configured for the Substrate interface. The [Bittensor Python SDK](https://github.com/opentensor/bittensor) provides typed wrappers; standard Polkadot.js and substrateinterface require the raw RPC interface (see SDK Integration tabs below).
{% endhint %}

Returns information for **all active subnets** on Bittensor — netUid, owner, hyperparameters, validators count, total stake, emission rate, and more. This is the canonical query for getting a network-wide picture of subnet activity.

## Parameters

| Parameter | Type   | Required | Description                                        |
| --------- | ------ | -------- | -------------------------------------------------- |
| blockHash | string | No       | Block hash to query at. Omit for the latest block. |

## Request Example

{% tabs %}
{% tab title="cURL" %}
```bash
curl --location --request POST 'https://go.getblock.io/<ACCESS-TOKEN>/' \
--header 'Content-Type: application/json' \
--data-raw '{
    "jsonrpc": "2.0",
    "method": "subnetInfo_getSubnetsInfo",
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
    "method": "subnetInfo_getSubnetsInfo",
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
    "method": "subnetInfo_getSubnetsInfo",
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
        "method": "subnetInfo_getSubnetsInfo",
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
    "result": [
        {
            "netuid": 1,
            "rho": 10,
            "kappa": 32767,
            "difficulty": 10000000,
            "immunityPeriod": 4096,
            "maxAllowedValidators": 64,
            "minAllowedWeights": 8,
            "maxWeightsLimit": 65535,
            "scalingLawPower": 50,
            "subnetworkN": 256,
            "maxAllowedUids": 4096,
            "blocksSinceLastStep": 12,
            "tempo": 360,
            "networkModality": 0,
            "networkConnect": [],
            "emissionValues": "0x...",
            "burn": "0x12a05f200",
            "owner": "5Hp1q5K7T8QYsxwBwYjqkZ1QzGqgYqU3pwMwYqDdjjEqLqJk"
        }
    ]
}
```
{% endcode %}

## Response Parameters

| Field                    | Type    | Description                                                         |
| ------------------------ | ------- | ------------------------------------------------------------------- |
| result                   | array   | Array of subnet info objects, one per active subnet                 |
| result\[].netuid         | integer | Subnet ID (numeric)                                                 |
| result\[].rho            | integer | Rho hyperparameter (consensus parameter)                            |
| result\[].kappa          | integer | Kappa hyperparameter (consensus parameter)                          |
| result\[].difficulty     | integer | Current registration difficulty (PoW for registration)              |
| result\[].tempo          | integer | Tempo — number of blocks between emission/scoring events            |
| result\[].maxAllowedUids | integer | Maximum number of neurons in the subnet                             |
| result\[].owner          | string  | SS58-encoded coldkey of the subnet owner (post-dTAO)                |
| result\[].burn           | string  | Current burn cost for registration via burning (alternative to PoW) |

## Use Cases

* Subnet directories and discovery UIs
* Network-wide analytics — total stake across all subnets, total emissions, etc.
* Choosing which subnets to participate in based on parameters

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
{% tab title="Bittensor SDK (Python)" %}
```python
import bittensor as bt

# Bittensor SDK provides typed wrappers for chain-specific data.
# The subtensor client routes to the appropriate custom RPC under the hood:
subtensor = bt.subtensor(network="finney")  # or pass GetBlock URL

# Example for subnetInfo_getSubnetInfo:
# subnet_info = subtensor.get_subnet_info(netuid=1)
# print(subnet_info)

# Or use the raw RPC interface for any custom method:
result = subtensor.substrate.rpc_request("subnetInfo_getSubnetsInfo", [])
print(result)
```
{% endtab %}

{% tab title="Polkadot.js raw (TypeScript)" %}
```javascript
import { ApiPromise, WsProvider } from '@polkadot/api';

const provider = new WsProvider('wss://go.getblock.io/<ACCESS-TOKEN>/');
const api = await ApiPromise.create({ provider });

// Bittensor-specific RPCs need the raw send() interface
// (they aren't part of standard Polkadot.js typed wrappers):
const result = await api.rpc.send({
    method: 'subnetInfo_getSubnetsInfo',
    params: []
});
console.log(result);
```
{% endtab %}
{% endtabs %}
