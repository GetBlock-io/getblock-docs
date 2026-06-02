---
description: >-
  Example code for the consensus_params JSON-RPC method. Complete guide on how
  to use consensus_params JSON-RPC in GetBlock Web3 documentation.
---

# consensus\_params - Cosmos

Returns the consensus parameters at a specific height — block size limits, evidence rules, and allowed validator public key types.

## Parameters

| Parameter | Type    | Required | Description                       |
| --------- | ------- | -------- | --------------------------------- |
| height    | integer | No       | Block height (defaults to latest) |

## Request Example

{% tabs %}
{% tab title="cURL" %}
```bash
# JSON-RPC over HTTP POST (canonical)
curl --location --request POST 'https://go.getblock.io/<ACCESS-TOKEN>/consensus_params' \
--header 'Content-Type: application/json' \
--data-raw '{
    "jsonrpc": "2.0",
    "method": "consensus_params",
    "params": {
        "height": 23456789
    },
    "id": "getblock.io"
}'

# URI over HTTP GET (alternate, for simple methods)
curl --location --request GET 'https://go.getblock.io/<ACCESS-TOKEN>/consensus_params?height=23456789'
```
{% endtab %}

{% tab title="JavaScript (Axios)" %}
```javascript
import axios from 'axios';

const data = JSON.stringify({
    "jsonrpc": "2.0",
    "method": "consensus_params",
    "params": {
        "height": 23456789
    },
    "id": "getblock.io"
});

const config = {
    method: 'post',
    url: 'https://go.getblock.io/<ACCESS-TOKEN>/consensus_params',
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

url = "https://go.getblock.io/<ACCESS-TOKEN>/consensus_params"

payload = json.dumps({
    "jsonrpc": "2.0",
    "method": "consensus_params",
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
        "method": "consensus_params",
        "params": {
                "height": 23456789
        },
        "id": "getblock.io"
});

    let response = client
        .post("https://go.getblock.io/<ACCESS-TOKEN>/consensus_params")
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
        "block_height": "23456789",
        "consensus_params": {
            "block": {
                "max_bytes": "22020096",
                "max_gas": "-1"
            },
            "evidence": {
                "max_age_num_blocks": "100000",
                "max_age_duration": "172800000000000",
                "max_bytes": "1048576"
            },
            "validator": {
                "pub_key_types": [
                    "ed25519"
                ]
            },
            "version": {
                "app": "0"
            }
        }
    }
}
```
{% endcode %}

## Response Parameters

| Field                                                   | Type            | Description                                  |
| ------------------------------------------------------- | --------------- | -------------------------------------------- |
| result.block\_height                                    | string          | Block height at which params were sampled    |
| result.consensus\_params.block.max\_bytes               | string          | Maximum block size in bytes                  |
| result.consensus\_params.block.max\_gas                 | string          | Maximum gas per block (`-1` means unlimited) |
| result.consensus\_params.evidence.max\_age\_num\_blocks | string          | Maximum block age for accepting evidence     |
| result.consensus\_params.validator.pub\_key\_types      | array of string | Allowed validator key types                  |

## Use Cases

* Detecting consensus parameter upgrades
* Sizing transaction batches to fit within block limits
* Compliance tooling that audits chain parameters over time

## Error Handling

| Status Code | Error Message     | Cause                                          |
| ----------- | ----------------- | ---------------------------------------------- |
| 404         | Not Found         | Missing or invalid `<ACCESS-TOKEN>`            |
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
const result = await httpClient.execute({"jsonrpc": "2.0", "id": "getblock.io", "method": "consensus_params", "params": {"height": 23456789}});
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
    'method': 'consensus_params',
    'params': {"height": 23456789}
}})
print(response.json())
```
{% endtab %}
{% endtabs %}
