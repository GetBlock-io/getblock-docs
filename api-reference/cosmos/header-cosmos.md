---
description: >-
  Example code for the header JSON-RPC method. Complete guide on how to use
  header JSON-RPC in GetBlock Web3 documentation.
---

# header - Cosmos

Returns the block header at a specific height. Lighter alternative to `block` when you only need header data (chain id, height, time, hashes) without the full transaction list.

## Parameters

| Parameter | Type    | Required | Description                                          |
| --------- | ------- | -------- | ---------------------------------------------------- |
| height    | integer | No       | Block height (decimal). Defaults to the latest block |

## Request Example

{% tabs %}
{% tab title="cURL" %}
```bash
# JSON-RPC over HTTP POST (canonical)
curl --location --request POST 'https://go.getblock.io/<ACCESS-TOKEN>/header' \
--header 'Content-Type: application/json' \
--data-raw '{
    "jsonrpc": "2.0",
    "method": "header",
    "params": {
        "height": 23456789
    },
    "id": "getblock.io"
}'

# URI over HTTP GET (alternate, for simple methods)
curl --location --request GET 'https://go.getblock.io/<ACCESS-TOKEN>/header?height=23456789'
```
{% endtab %}

{% tab title="JavaScript (Axios)" %}
```javascript
import axios from 'axios';

const data = JSON.stringify({
    "jsonrpc": "2.0",
    "method": "header",
    "params": {
        "height": 23456789
    },
    "id": "getblock.io"
});

const config = {
    method: 'post',
    url: 'https://go.getblock.io/<ACCESS-TOKEN>/header',
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

url = "https://go.getblock.io/<ACCESS-TOKEN>/header"

payload = json.dumps({
    "jsonrpc": "2.0",
    "method": "header",
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
        "method": "header",
        "params": {
                "height": 23456789
        },
        "id": "getblock.io"
});

    let response = client
        .post("https://go.getblock.io/<ACCESS-TOKEN>/header")
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
        "header": {
            "version": {
                "block": "11",
                "app": "0"
            },
            "chain_id": "cosmoshub-4",
            "height": "23456789",
            "time": "2026-05-26T13:42:18.5273Z",
            "last_block_id": {
                "hash": "..."
            },
            "data_hash": "F8C44C8E2E4E8C0F2B7A8E6D4C2E0F1A3B5C7D9E",
            "validators_hash": "...",
            "next_validators_hash": "...",
            "consensus_hash": "...",
            "app_hash": "...",
            "proposer_address": "8E2B7A..."
        }
    }
}
```
{% endcode %}

## Response Parameters

| Field                           | Type   | Description                                    |
| ------------------------------- | ------ | ---------------------------------------------- |
| result.header.chain\_id         | string | Chain ID                                       |
| result.header.height            | string | Block height                                   |
| result.header.time              | string | ISO 8601 block timestamp                       |
| result.header.data\_hash        | string | Hash of the block's transactions               |
| result.header.app\_hash         | string | Hash of the application state after this block |
| result.header.proposer\_address | string | Validator address that proposed this block     |

## Use Cases

* Lightweight chain-tip polling without fetching full block bodies
* Header-only indexing in resource-constrained environments
* Detecting reorgs by header chain reconciliation

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

const httpClient = new HttpClient('https://go.getblock.io/<ACCESS-TOKEN>/header');
const client = await Tendermint37Client.create(httpClient);

// CosmJS exposes typed methods on the client (e.g. client.status(), client.block()).
// For raw access to any RPC method:
const result = await httpClient.execute({"jsonrpc": "2.0", "id": "getblock.io", "method": "header", "params": {"height": 23456789}});
console.log(result);
```
{% endtab %}

{% tab title="cosmpy / requests" %}
```python
# cosmpy wraps Cosmos REST / gRPC. For raw CometBFT RPC access, use requests.
import requests

response = requests.post('https://go.getblock.io/<ACCESS-TOKEN>/header', json={
    'jsonrpc': '2.0',
    'id': 'getblock.io',
    'method': 'header',
    'params': {"height": 23456789}
}})
print(response.json())
```
{% endtab %}
{% endtabs %}
