# neuroninfo\_getneurons bittensor

{% hint style="info" %}
**Bittensor-specific custom RPC method.** This method is implemented by the Subtensor runtime and is not part of the standard Substrate JSON-RPC surface. Call against a GetBlock endpoint configured for the Substrate interface. The [Bittensor Python SDK](https://github.com/opentensor/bittensor) provides typed wrappers; standard Polkadot.js and substrateinterface require the raw RPC interface (see SDK Integration tabs below).
{% endhint %}

Returns the full neuron list (metagraph) for a given subnet — every registered neuron's hotkey, coldkey, stake, rank, consensus, trust, incentive, dividends, emission, and serving endpoint (Axon/Prometheus). This is the canonical metagraph query.

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
    "method": "neuronInfo_getNeurons",
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
    "method": "neuronInfo_getNeurons",
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
    "method": "neuronInfo_getNeurons",
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
        "method": "neuronInfo_getNeurons",
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

{% code overflow="wrap" %}
```json
{
    "jsonrpc": "2.0",
    "id": "getblock.io",
    "result": [
        {
            "hotkey": "5DAAnrj7VHTznn2AWBemMuyBwZWs6FNFjdyVXUeYum3PTXFy",
            "coldkey": "5Hp1q5K7T8QYsxwBwYjqkZ1QzGqgYqU3pwMwYqDdjjEqLqJk",
            "uid": 0,
            "netuid": 1,
            "active": true,
            "axonInfo": {
                "block": 5870042,
                "version": 220,
                "ip": 0,
                "port": 8091,
                "ipType": 4,
                "protocol": 0,
                "placeholder1": 0,
                "placeholder2": 0
            },
            "prometheusInfo": {
                "block": 5870042,
                "version": 220,
                "ip": 0,
                "port": 7091,
                "ipType": 4
            },
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
            "validatorTrust": 7654,
            "dividends": 1234,
            "lastUpdate": 5870030,
            "validatorPermit": true,
            "weights": [
                [
                    1,
                    65535
                ],
                [
                    2,
                    0
                ]
            ],
            "bonds": [
                [
                    0,
                    0
                ],
                [
                    1,
                    32768
                ]
            ],
            "pruningScore": 0
        }
    ]
}
```
{% endcode %}

## Response Parameters

| Field                     | Type    | Description                                                      |
| ------------------------- | ------- | ---------------------------------------------------------------- |
| result                    | array   | Array of neuron objects, one per registered neuron in the subnet |
| result\[].hotkey          | string  | SS58-encoded hotkey (signing key for validator/miner ops)        |
| result\[].coldkey         | string  | SS58-encoded coldkey (owns the hotkey, holds stake)              |
| result\[].uid             | integer | Neuron UID within the subnet (0 to maxAllowedUids - 1)           |
| result\[].stake           | array   | Stake breakdown — array of \[coldkey, amount] pairs              |
| result\[].rank            | integer | Neuron rank (scaled 0..65535)                                    |
| result\[].incentive       | integer | Incentive score (scaled 0..65535)                                |
| result\[].consensus       | integer | Consensus score (scaled 0..65535)                                |
| result\[].trust           | integer | Trust score (scaled 0..65535)                                    |
| result\[].emission        | string  | Emission earned by this neuron in the last tempo, in rao         |
| result\[].validatorPermit | boolean | Whether this neuron has validator permit                         |
| result\[].axonInfo        | object  | Axon serving endpoint info — IP, port, version, protocol         |
| result\[].weights         | array   | Outgoing weights this neuron has set on other neurons            |
| result\[].bonds           | array   | EMA bonds this neuron holds with other neurons                   |

## Use Cases

* Building metagraph explorers and subnet dashboards
* Detecting which neurons are scoring highly (high incentive / consensus)
* Reading validator weights for incentive mechanism analysis
* Tracking historical neuron registration and ranking changes

## Error Handling

| Status Code | Error Message     | Cause                                                                                                                      |
| ----------- | ----------------- | -------------------------------------------------------------------------------------------------------------------------- |
| 403         | Forbidden         | Missing or invalid `<ACCESS-TOKEN>`                                                                                        |
| -32602      | Invalid params    | Request parameters are missing or malformed                                                                                |
| -32601      | Method not found  | Method does not exist on this interface — check whether you're calling a Substrate method on an EVM endpoint or vice versa |
| -32603      | Internal error    | Server-side error while processing the request                                                                             |
| 429         | Too Many Requests | Rate limit exceeded for your plan                                                                                          |
| -32602      | Subnet not found  | No subnet exists at the given netuid                                                                                       |

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
result = subtensor.substrate.rpc_request("neuronInfo_getNeurons", [1])
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
    method: 'neuronInfo_getNeurons',
    params: [1]
});
console.log(result);
```
{% endtab %}
{% endtabs %}
