---
description: >-
  Example code for the block_results JSON-RPC method. Complete guide on how to
  use block_results JSON-RPC in GetBlock Web3 documentation.
---

# block\_results - Cosmos

Returns the results of all transactions executed in a block — ABCI codes, gas used, events emitted, and per-transaction data. This is where event-driven indexers extract structured information about what happened in a block.

## Parameters

| Parameter | Type    | Required | Description                                                     |
| --------- | ------- | -------- | --------------------------------------------------------------- |
| height    | integer | No       | Block height (decimal). Defaults to the latest block if omitted |

## Request Example

{% tabs %}
{% tab title="cURL" %}
```bash
# JSON-RPC over HTTP POST (canonical)
curl --location --request POST 'https://go.getblock.io/<ACCESS-TOKEN>/block_results' \
--header 'Content-Type: application/json' \
--data-raw '{
    "jsonrpc": "2.0",
    "method": "block_results",
    "params": {
        "height": 23456789
    },
    "id": "getblock.io"
}'

# URI over HTTP GET (alternate, for simple methods)
curl --location --request GET 'https://go.getblock.io/<ACCESS-TOKEN>/block_results?height=23456789'
```
{% endtab %}

{% tab title="JavaScript (Axios)" %}
```javascript
import axios from 'axios';

const data = JSON.stringify({
    "jsonrpc": "2.0",
    "method": "block_results",
    "params": {
        "height": 23456789
    },
    "id": "getblock.io"
});

const config = {
    method: 'post',
    url: 'https://go.getblock.io/<ACCESS-TOKEN>/block_results',
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

url = "https://go.getblock.io/<ACCESS-TOKEN>/block_results"

payload = json.dumps({
    "jsonrpc": "2.0",
    "method": "block_results",
    "params": {
        "height": 23456789
    },
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
        "method": "block_results",
        "params": {
                "height": 23456789
        },
        "id": "getblock.io"
});

    let response = client
        .post("https://go.getblock.io/<ACCESS-TOKEN>/block_results")
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
        "height": "23456789",
        "txs_results": [
            {
                "code": 0,
                "data": "EisKKS9jb3Ntb3MuYmFuay52MWJldGExLk1zZ1NlbmRSZXNwb25zZQ==",
                "log": "[{\"events\":[{\"type\":\"transfer\",\"attributes\":[...]}]}]",
                "gas_wanted": "200000",
                "gas_used": "67890",
                "events": [
                    {
                        "type": "transfer",
                        "attributes": [
                            {
                                "key": "recipient",
                                "value": "cosmos1ghd753shjecj4iv2x2i8xrkzzanzjvvc22x2kn"
                            },
                            {
                                "key": "sender",
                                "value": "cosmos1t6rlgf80jd8tanpnnvmmxfqdcm9cn7wctrs7hk"
                            },
                            {
                                "key": "amount",
                                "value": "100000uatom"
                            }
                        ]
                    }
                ]
            }
        ],
        "finalize_block_events": [],
        "validator_updates": [],
        "consensus_param_updates": {
            "block": {
                "max_bytes": "22020096",
                "max_gas": "-1"
            },
            "evidence": {
                "max_age_num_blocks": "100000"
            },
            "validator": {
                "pub_key_types": [
                    "ed25519"
                ]
            }
        }
    }
}
```

## Response Parameters

| Field                              | Type    | Description                                                            |
| ---------------------------------- | ------- | ---------------------------------------------------------------------- |
| result.height                      | string  | Block height (echoed)                                                  |
| result.txs\_results                | array   | Per-transaction execution results                                      |
| result.txs\_results\[].code        | integer | ABCI response code: `0` for success, non-zero for failure              |
| result.txs\_results\[].gas\_wanted | string  | Gas limit declared by the transaction                                  |
| result.txs\_results\[].gas\_used   | string  | Actual gas consumed                                                    |
| result.txs\_results\[].events      | array   | Events emitted by the transaction — the primary event-sourcing surface |
| result.finalize\_block\_events     | array   | Events emitted by the BeginBlock/EndBlock app phases                   |
| result.validator\_updates          | array   | Validator set changes applied at this block                            |

## Use Cases

* Event-driven indexers extracting transfers, delegations, IBC packets, etc.
* Computing per-transaction gas-cost analytics
* Detecting validator set updates and slashings
* Building application-level explorers

## Error Handling

| Status Code | Error Message     | Cause                                          |
| ----------- | ----------------- | ---------------------------------------------- |
| 404         | Not found         | Missing or invalid `<ACCESS-TOKEN>`            |
| -32602      | Invalid params    | Request parameters are missing or malformed    |
| -32603      | Internal error    | Server-side error while processing the request |
| 429         | Too Many Requests | Rate limit exceeded for your plan              |

## SDK Integration

{% tabs %}
{% tab title="CosmJS" %}
```javascript
import { Tendermint37Client, HttpClient } from '@cosmjs/tendermint-rpc';

const httpClient = new HttpClient('https://go.getblock.io/<ACCESS-TOKEN>/');
const client = await Tendermint37Client.create(httpClient);

// CosmJS exposes typed methods on the client (e.g. client.status(), client.block()).
// For raw access to any RPC method:
const result = await httpClient.execute({"jsonrpc": "2.0", "id": "getblock.io", "method": "block_results", "params": {"height": 23456789}});
console.log(result);
```
{% endtab %}

{% tab title="cosmpy / requests" %}
```python
# cosmpy wraps Cosmos REST / gRPC. For raw CometBFT RPC access, use requests.
import requests

response = requests.post('https://go.getblock.io/<ACCESS-TOKEN>/', json={
    'jsonrpc': '2.0',
    'id': 'getblock.io',
    'method': 'block_results',
    'params': {"height": 23456789}
}})
print(response.json())
```
{% endtab %}
{% endtabs %}
