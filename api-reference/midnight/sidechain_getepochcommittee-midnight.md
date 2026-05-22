---
description: >-
  Example code for the sidechain_getEpochCommittee JSON-RPC method. Complete
  guide on how to use sidechain_getEpochCommittee JSON-RPC in GetBlock Web3
  documentation.
---

# sidechain\_getEpochCommittee - Midnight

This method returns the validator committee for a given epoch — the set of authorities that produce and finalize blocks during that epoch.

## Parameters

| Parameter   | Type    | Required | Description                                          |
| ----------- | ------- | -------- | ---------------------------------------------------- |
| epochNumber | integer | No       | Epoch number to query. Defaults to the current epoch |

## Request Example

{% tabs %}
{% tab title="cURL" %}
```bash
curl --location --request POST 'https://go.getblock.io/<ACCESS-TOKEN>/' \
--header 'Content-Type: application/json' \
--data-raw '{
    "jsonrpc": "2.0",
    "method": "sidechain_getEpochCommittee",
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
    "method": "sidechain_getEpochCommittee",
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
    "method": "sidechain_getEpochCommittee",
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
        "method": "sidechain_getEpochCommittee",
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
        "epoch": 42,
        "committee": [
            {
                "sidechainPubKey": "0x02\u2026abc",
                "stakePoolPubKey": "0xed\u2026def"
            }
        ]
    }
}
```
{% endcode %}

## Response Parameters

| Field                               | Type            | Description                                    |
| ----------------------------------- | --------------- | ---------------------------------------------- |
| result.epoch                        | integer         | Epoch number this committee applies to         |
| result.committee                    | array of object | Validator entries                              |
| result.committee\[].sidechainPubKey | string          | Midnight sidechain public key of the validator |
| result.committee\[].stakePoolPubKey | string          | Cardano stake pool public key of the validator |

## Use Cases

* Validator monitoring and performance tracking
* Detecting committee rotation across epochs
* Audits of validator participation

## Error Handling

| Status Code | Error Message     | Cause                                       |
| ----------- | ----------------- | ------------------------------------------- |
| 403         | Forbidden         | Missing or invalid `<ACCESS-TOKEN>`         |
| -32602      | Invalid params    | Request parameters are missing or malformed |
| 429         | Too Many Requests | Rate limit exceeded for your plan           |

## SDK Integration

{% tabs %}
{% tab title="@polkadot/api (JavaScript)" %}
{% code overflow="wrap" %}
```javascript
import { ApiPromise, WsProvider } from '@polkadot/api';

const wsProvider = new WsProvider('wss://go.getblock.io/<ACCESS-TOKEN>/');
const api = await ApiPromise.create({ provider: wsProvider });

// Call via the typed API:
const result = await api.rpc.sidechain.getEpochCommittee();
console.log(result.toJSON());

// Or call the raw JSON-RPC method:
const raw = await api.rpc.rpc.methods();
// const raw = await (api as any)._rpcCore.provider.send('sidechain_getEpochCommittee', []);
```
{% endcode %}
{% endtab %}

{% tab title="substrate-interface (Python)" %}
```python
from substrateinterface import SubstrateInterface

substrate = SubstrateInterface(url='wss://go.getblock.io/<ACCESS-TOKEN>/')

# Generic RPC call:
result = substrate.rpc_request('sidechain_getEpochCommittee', [])
print(result)
```
{% endtab %}
{% endtabs %}
