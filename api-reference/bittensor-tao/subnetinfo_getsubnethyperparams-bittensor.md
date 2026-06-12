# subnetinfo\_getsubnethyperparams bittensor

{% hint style="info" %}
**Bittensor-specific custom RPC method.** This method is implemented by the Subtensor runtime and is not part of the standard Substrate JSON-RPC surface. Call against a GetBlock endpoint configured for the Substrate interface. The [Bittensor Python SDK](https://github.com/opentensor/bittensor) provides typed wrappers; standard Polkadot.js and substrateinterface require the raw RPC interface (see SDK Integration tabs below).
{% endhint %}

Returns just the hyperparameters governing a subnet's behavior — separated from general info for clarity. Useful for showing detailed consensus tuning data.

## Parameters

| Parameter | Type    | Required | Description            |
| --------- | ------- | -------- | ---------------------- |
| netuid    | integer | Yes      | Subnet ID              |
| blockHash | string  | No       | Block hash to query at |

## Request Example

{% tabs %}
{% tab title="cURL" %}
```bash
curl --location --request POST 'https://go.getblock.io/<ACCESS-TOKEN>/' \
--header 'Content-Type: application/json' \
--data-raw '{
    "jsonrpc": "2.0",
    "method": "subnetInfo_getSubnetHyperparams",
    "params": [
        1
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
    "method": "subnetInfo_getSubnetHyperparams",
    "params": [
        1
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
    "method": "subnetInfo_getSubnetHyperparams",
    "params": [
        1
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
        "method": "subnetInfo_getSubnetHyperparams",
        "params": [
                1
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
        "rho": 10,
        "kappa": 32767,
        "immunityPeriod": 4096,
        "minAllowedWeights": 8,
        "maxWeightsLimit": 65535,
        "tempo": 360,
        "minDifficulty": 10000000,
        "maxDifficulty": 9223372036854775807,
        "weightsVersion": 0,
        "weightsRateLimit": 100,
        "adjustmentInterval": 100,
        "activityCutoff": 5000,
        "registrationAllowed": true,
        "targetRegsPerInterval": 2,
        "minBurn": 1000000000,
        "maxBurn": 100000000000,
        "bondsMovingAvg": 900000,
        "maxRegsPerBlock": 1,
        "servingRateLimit": 50
    }
}
```

## Response Parameters

| Field                      | Type    | Description                                                      |
| -------------------------- | ------- | ---------------------------------------------------------------- |
| result.tempo               | integer | Blocks between emission events                                   |
| result.immunityPeriod      | integer | Blocks during which a newly-registered neuron cannot be replaced |
| result.minAllowedWeights   | integer | Minimum number of weight assignments a validator must submit     |
| result.adjustmentInterval  | integer | Blocks between difficulty adjustments                            |
| result.activityCutoff      | integer | Blocks of inactivity before a neuron is considered inactive      |
| result.minBurn             | integer | Minimum registration burn cost (in rao)                          |
| result.maxBurn             | integer | Maximum registration burn cost (in rao)                          |
| result.registrationAllowed | boolean | Whether new registrations are currently allowed on this subnet   |

## Use Cases

* Subnet configuration dashboards
* Detecting subnet parameter changes (subnets can update hyperparams via subnet owner extrinsics)
* Helping subnet operators tune their participation strategy

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
result = subtensor.substrate.rpc_request("subnetInfo_getSubnetHyperparams", [1])
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
    method: 'subnetInfo_getSubnetHyperparams',
    params: [1]
});
console.log(result);
```
{% endtab %}
{% endtabs %}
