# delegateinfo\_getdelegates bittensor

{% hint style="info" %}
**Bittensor-specific custom RPC method.** This method is implemented by the Subtensor runtime and is not part of the standard Substrate JSON-RPC surface. Call against a GetBlock endpoint configured for the Substrate interface. The [Bittensor Python SDK](https://github.com/opentensor/bittensor) provides typed wrappers; standard Polkadot.js and substrateinterface require the raw RPC interface (see SDK Integration tabs below).
{% endhint %}

Returns all delegates available for TAO staking — hotkey, owner coldkey, take percentage, total stake delegated, validator permits per subnet, and more. Delegates are validators who accept stake from other TAO holders in exchange for a take of the rewards.

## Parameters

| Parameter | Type   | Required | Description            |
| --------- | ------ | -------- | ---------------------- |
| blockHash | string | No       | Block hash to query at |

## Request Example

{% tabs %}
{% tab title="cURL" %}
```bash
curl --location --request POST 'https://go.getblock.io/<ACCESS-TOKEN>/' \
--header 'Content-Type: application/json' \
--data-raw '{
    "jsonrpc": "2.0",
    "method": "delegateInfo_getDelegates",
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
    "method": "delegateInfo_getDelegates",
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
    "method": "delegateInfo_getDelegates",
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
        "method": "delegateInfo_getDelegates",
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
            "delegateSs58": "5GpwjAKpRJ3JpDLvuLn8KGkbSYzZHC9YMVtMs7vCYK4tFKfP",
            "take": 11796,
            "nominators": [
                [
                    "5Hp1q5K7T8QYsxwBwYjqkZ1QzGqgYqU3pwMwYqDdjjEqLqJk",
                    "5000000000000"
                ]
            ],
            "ownerSs58": "5Hp1q5K7T8QYsxwBwYjqkZ1QzGqgYqU3pwMwYqDdjjEqLqJk",
            "registrations": [
                1,
                2,
                5,
                8,
                11
            ],
            "validatorPermits": [
                1,
                5,
                8
            ],
            "returnPer1000": "12345678",
            "totalDailyReturn": "98765432100"
        }
    ]
}
```
{% endcode %}

## Response Parameters

| Field                      | Type             | Description                                                                  |
| -------------------------- | ---------------- | ---------------------------------------------------------------------------- |
| result                     | array            | Array of delegate objects, one per registered delegate                       |
| result\[].delegateSs58     | string           | SS58-encoded delegate hotkey                                                 |
| result\[].ownerSs58        | string           | SS58-encoded owner coldkey                                                   |
| result\[].take             | integer          | Take percentage (scaled — divide by 65535 to get fraction; e.g. 11796 ≈ 18%) |
| result\[].nominators       | array            | Array of \[coldkey, stake\_amount\_in\_rao] pairs                            |
| result\[].registrations    | array of integer | Subnets where this delegate is registered as a neuron                        |
| result\[].validatorPermits | array of integer | Subnets where this delegate has validator permit                             |
| result\[].returnPer1000    | string           | Return per 1000 TAO staked (estimated APR proxy, in rao)                     |
| result\[].totalDailyReturn | string           | Total daily return earned, in rao                                            |

## Use Cases

* Staking UIs showing all available delegates with APR estimates
* Delegate leaderboards (ranked by total stake or returns)
* Pre-stake decision support — comparing take percentages and subnet diversity

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
result = subtensor.substrate.rpc_request("delegateInfo_getDelegates", [])
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
    method: 'delegateInfo_getDelegates',
    params: []
});
console.log(result);
```
{% endtab %}
{% endtabs %}
