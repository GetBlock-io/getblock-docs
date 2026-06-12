# neuroninfo\_getneuron bittensor

{% hint style="info" %}
**Bittensor-specific custom RPC method.** This method is implemented by the Subtensor runtime and is not part of the standard Substrate JSON-RPC surface. Call against a GetBlock endpoint configured for the Substrate interface. The [Bittensor Python SDK](https://github.com/opentensor/bittensor) provides typed wrappers; standard Polkadot.js and substrateinterface require the raw RPC interface (see SDK Integration tabs below).
{% endhint %}

Returns information for a **single neuron** by subnet netUid and neuron UID. Same response shape as one element of `neuronInfo_getNeurons`. Cheaper than fetching the full metagraph when you only need one neuron.

## Parameters

| Parameter | Type    | Required | Description                  |
| --------- | ------- | -------- | ---------------------------- |
| netuid    | integer | Yes      | Subnet ID                    |
| uid       | integer | Yes      | Neuron UID within the subnet |
| blockHash | string  | No       | Block hash to query at       |

## Request Example

{% tabs %}
{% tab title="cURL" %}
```bash
curl --location --request POST 'https://go.getblock.io/<ACCESS-TOKEN>/' \
--header 'Content-Type: application/json' \
--data-raw '{
    "jsonrpc": "2.0",
    "method": "neuronInfo_getNeuron",
    "params": [
        1,
        0
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
    "method": "neuronInfo_getNeuron",
    "params": [
        1,
        0
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
    "method": "neuronInfo_getNeuron",
    "params": [
        1,
        0
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
        "method": "neuronInfo_getNeuron",
        "params": [
                1,
                0
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
        "hotkey": "5DAAnrj7VHTznn2AWBemMuyBwZWs6FNFjdyVXUeYum3PTXFy",
        "coldkey": "5Hp1q5K7T8QYsxwBwYjqkZ1QzGqgYqU3pwMwYqDdjjEqLqJk",
        "uid": 0,
        "netuid": 1,
        "active": true,
        "stake": [
            [
                "5Hp1q5K7T8QYsxwBwYjqkZ1QzGqgYqU3pwMwYqDdjjEqLqJk",
                "1000000000000"
            ]
        ],
        "rank": 8765,
        "emission": "12345678",
        "incentive": 9876,
        "consensus": 5432,
        "trust": 6543,
        "validatorPermit": true
    }
}
```
{% endcode %}

## Response Parameters

| Field  | Type           | Description                                                           |
| ------ | -------------- | --------------------------------------------------------------------- |
| result | object \| null | Neuron data, or `null` if no neuron exists at the given (netuid, uid) |

## Use Cases

* Neuron detail pages — `taostats.io`-style explorers
* Tracking a specific neuron's score over time
* Pre-flight checks before submitting set\_weights extrinsics

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
result = subtensor.substrate.rpc_request("neuronInfo_getNeuron", [1, 0])
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
    method: 'neuronInfo_getNeuron',
    params: [1, 0]
});
console.log(result);
```
{% endtab %}
{% endtabs %}
