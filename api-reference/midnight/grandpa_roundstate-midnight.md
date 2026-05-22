---
description: >-
  Example code for the grandpa_roundState JSON-RPC method. Complete guide on how
  to use grandpa_roundState JSON-RPC in GetBlock Web3 documentation.
---

# grandpa\_roundState - Midnight

This method returns the current GRANDPA round state, including round number, voter set, and prevote/precommit progress. Useful for monitoring finality health.

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
    "method": "grandpa_roundState",
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
    "method": "grandpa_roundState",
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
    "method": "grandpa_roundState",
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
        "method": "grandpa_roundState",
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
        "setId": 1024,
        "best": {
            "round": 1234,
            "totalWeight": 100,
            "thresholdWeight": 67,
            "prevotes": {
                "currentWeight": 80,
                "missing": []
            },
            "precommits": {
                "currentWeight": 75,
                "missing": []
            }
        },
        "background": []
    }
}
```
{% endcode %}

## Response Parameters

| Field                       | Type    | Description                           |
| --------------------------- | ------- | ------------------------------------- |
| result.setId                | integer | Current GRANDPA voter set ID          |
| result.best.round           | integer | Current GRANDPA round number          |
| result.best.totalWeight     | integer | Total voter weight in the round       |
| result.best.thresholdWeight | integer | Weight threshold required to finalize |
| result.best.prevotes        | object  | Prevote participation state           |
| result.best.precommits      | object  | Precommit participation state         |

## Use Cases

* Validator monitoring dashboards
* Detecting stalled finality
* Analytics on GRANDPA participation rates

## Error Handling

| Status Code | Error Message     | Cause                                       |
| ----------- | ----------------- | ------------------------------------------- |
| 403         | Forbidden         | Missing or invalid `<ACCESS-TOKEN>`         |
| -32602      | Invalid params    | Request parameters are missing or malformed |
| 429         | Too Many Requests | Rate limit exceeded for your plan           |

## SDK Integration

{% tabs %}
{% tab title="@polkadot/api (JavaScript)" %}
```javascript
import { ApiPromise, WsProvider } from '@polkadot/api';

const wsProvider = new WsProvider('wss://go.getblock.io/<ACCESS-TOKEN>/');
const api = await ApiPromise.create({ provider: wsProvider });

// Call via the typed API:
const result = await api.rpc.grandpa.roundState();
console.log(result.toJSON());

// Or call the raw JSON-RPC method:
const raw = await api.rpc.rpc.methods();
// const raw = await (api as any)._rpcCore.provider.send('grandpa_roundState', []);
```
{% endtab %}

{% tab title="substrate-interface (Python)" %}
```python
from substrateinterface import SubstrateInterface

substrate = SubstrateInterface(url='wss://go.getblock.io/<ACCESS-TOKEN>/')

# Generic RPC call:
result = substrate.rpc_request('grandpa_roundState', [])
print(result)
```
{% endtab %}
{% endtabs %}
