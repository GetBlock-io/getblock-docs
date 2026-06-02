---
description: >-
  Example code for the validators JSON-RPC method. Complete guide on how to use
  validators JSON-RPC in GetBlock Web3 documentation.
---

# validators - Cosmos

Returns the validator set at a specific height. The set is paginated — use `page` and `per_page` to traverse beyond the first results.

## Parameters

| Parameter | Type    | Required | Description                                 |
| --------- | ------- | -------- | ------------------------------------------- |
| height    | integer | No       | Block height (defaults to latest)           |
| page      | integer | No       | Page number (default: 1)                    |
| per\_page | integer | No       | Validators per page (default: 30, max: 100) |

## Request Example

{% tabs %}
{% tab title="cURL" %}
```bash
# JSON-RPC over HTTP POST (canonical)
curl --location --request POST 'https://go.getblock.io/<ACCESS-TOKEN>/validators' \
--header 'Content-Type: application/json' \
--data-raw '{
    "jsonrpc": "2.0",
    "method": "validators",
    "params": {
        "height": 23456789,
        "page": 1,
        "per_page": 30
    },
    "id": "getblock.io"
}'

# URI over HTTP GET (alternate, for simple methods)
curl --location --request GET 'https://go.getblock.io/<ACCESS-TOKEN>/validators?height=23456789&page=1&per_page=30'
```
{% endtab %}

{% tab title="JavaScript (Axios)" %}
```javascript
import axios from 'axios';

const data = JSON.stringify({
    "jsonrpc": "2.0",
    "method": "validators",
    "params": {
        "height": 23456789,
        "page": 1,
        "per_page": 30
    },
    "id": "getblock.io"
});

const config = {
    method: 'post',
    url: 'https://go.getblock.io/<ACCESS-TOKEN>/validators',
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

url = "https://go.getblock.io/<ACCESS-TOKEN>/validators"

payload = json.dumps({
    "jsonrpc": "2.0",
    "method": "validators",
    "params": {
        "height": 23456789,
        "page": 1,
        "per_page": 30
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
        "method": "validators",
        "params": {
                "height": 23456789,
                "page": 1,
                "per_page": 30
        },
        "id": "getblock.io"
});

    let response = client
        .post("https://go.getblock.io/<ACCESS-TOKEN>/validators")
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
    "id": -1,
    "result": {
        "block_height": "31378890",
        "validators": [
            {
                "address": "25445D0EB353E9050AB11EC6197D5DCB611986DB",
                "pub_key": {
                    "type": "tendermint/PubKeyEd25519",
                    "value": "GUqbUS+7PeplNfxVcnDIyrXTLJPNdJQ4MQ6kBg5eYpw="
                },
                "voting_power": "7580387",
                "proposer_priority": "121437560"
            },
            {
                "address": "7B3A2EFE5B3FCDF819FCF52607314CEFE4754BB6",
                "pub_key": {
                    "type": "tendermint/PubKeyEd25519",
                    "value": "11pGwt6bot1EC5xeug8mulFNBBWsHV+X7XrxLUmTNF8="
                },
                "voting_power": "7397622",
                "proposer_priority": "-105504611"
            },
           
            {
                "address": "AC2D56057CD84765E6FBE318979093E8E44AA18F",
                "pub_key": {
                    "type": "tendermint/PubKeyEd25519",
                    "value": "0kNlxBMpm+5WtfHIG1xsWatOXTKPLtmSqn3EiEIDZeI="
                },
                "voting_power": "4963973",
                "proposer_priority": "-115852552"
            },
            {
                "address": "019B9CA2944D3CC36C7C73283EF3D58E56C8A5D4",
                "pub_key": {
                    "type": "tendermint/PubKeyEd25519",
                    "value": "pTcg/QVBC+LzpR0BuynDpMkNh4tajyeKCOtmWMfkvFU="
                },
                "voting_power": "4545831",
                "proposer_priority": "-25511373"
            },
            {
                "address": "679B89785973BE94D4FDF8B66F84A929932E91C5",
                "pub_key": {
                    "type": "tendermint/PubKeyEd25519",
                    "value": "Roh99RlsnDKHUFYUcQVHk2S84NeZfZdpc+CBb6NREhM="
                },
                "voting_power": "3215183",
                "proposer_priority": "174474300"
            }
        ],
        "count": "30",
        "total": "180"
    }
}
```
{% endcode %}

## Response Parameters

| Field                                   | Data Type | Description                                         |
| --------------------------------------- | --------- | --------------------------------------------------- |
| result.block\_height                    | string    | Block height at which the validator set was sampled |
| result.validators                       | array     | Validator entries                                   |
| result.validators\[].address            | string    | Validator's consensus address (20 bytes hex)        |
| result.validators\[].pub\_key.value     | string    | Base64-encoded Ed25519 public key                   |
| result.validators\[].voting\_power      | string    | Voting power (proportional to staked ATOM)          |
| result.validators\[].proposer\_priority | string    | Priority for the next block proposer rotation       |
| result.count                            | string    | Number of validators in this page                   |
| result.total                            | string    | Total active validators                             |

## Use Cases

* Validator monitoring dashboards
* Computing total voting power and validator concentration
* Detecting validator set changes over time
* Light client validator set verification

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
const result = await httpClient.execute({"jsonrpc": "2.0", "id": "getblock.io", "method": "validators", "params": {"height": 23456789, "page": 1, "per_page": 30}});
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
    'method': 'validators',
    'params': {"height": 23456789, "page": 1, "per_page": 30}
}})
print(response.json())
```
{% endtab %}
{% endtabs %}
