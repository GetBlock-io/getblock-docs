---
description: >-
  Example code for the abci_query JSON-RPC method. Complete guide on how to use
  abci_query JSON-RPC in GetBlock Web3 documentation.
---

# abci\_query - Cosmos

Runs an ABCI Query against the application — the underlying primitive that powers the Cosmos REST / LCD API and Cosmos SDK gRPC queries. Used to read module state directly. The `path` is the gRPC-style query path (e.g. `/cosmos.bank.v1beta1.Query/Balance`).

## Parameters

| Parameter | Type    | Required | Description                                                    |
| --------- | ------- | -------- | -------------------------------------------------------------- |
| path      | string  | Yes      | Query path (e.g. `/cosmos.bank.v1beta1.Query/Balance`)         |
| data      | string  | Yes      | Hex-encoded query request bytes (typically a Protobuf message) |
| height    | integer | No       | Block height for the query (default: latest)                   |
| prove     | boolean | No       | Include Merkle proof of the result (default: false)            |

## Request Example

{% tabs %}
{% tab title="cURL" %}
```bash
# JSON-RPC over HTTP POST (canonical)
curl --location --request POST 'https://go.getblock.io/<ACCESS-TOKEN>/' \
--header 'Content-Type: application/json' \
--data-raw '{
    "jsonrpc": "2.0",
    "method": "abci_query",
    "params": {
        "path": "/cosmos.bank.v1beta1.Query/Balance",
        "data": "0a2d636f736d6f73317436726c676638306a6438",
        "prove": false
    },
    "id": "getblock.io"
}'

# URI over HTTP GET (alternate, for simple methods)
curl --location --request GET 'https://go.getblock.io/<ACCESS-TOKEN>/abci_query?path="/cosmos.bank.v1beta1.Query/Balance"&data="0a2d636f736d6f73317436726c676638306a6438"&prove=false'
```
{% endtab %}

{% tab title="JavaScript (Axios)" %}
```javascript
import axios from 'axios';

const data = JSON.stringify({
    "jsonrpc": "2.0",
    "method": "abci_query",
    "params": {
        "path": "/cosmos.bank.v1beta1.Query/Balance",
        "data": "0a2d636f736d6f73317436726c676638306a6438",
        "prove": false
    },
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
    "method": "abci_query",
    "params": {
        "path": "/cosmos.bank.v1beta1.Query/Balance",
        "data": "0a2d636f736d6f73317436726c676638306a6438",
        "prove": false
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
        "method": "abci_query",
        "params": {
                "path": "/cosmos.bank.v1beta1.Query/Balance",
                "data": "0a2d636f736d6f73317436726c676638306a6438",
                "prove": false
        },
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
        "response": {
            "code": 0,
            "log": "",
            "info": "",
            "index": "0",
            "key": null,
            "value": "0a100a05756174...",
            "proof_ops": null,
            "height": "23456789",
            "codespace": ""
        }
    }
}
```
{% endcode %}

## Response Parameters

| Field                      | Type           | Description                                              |
| -------------------------- | -------------- | -------------------------------------------------------- |
| result.response.code       | integer        | `0` on success, non-zero on failure                      |
| result.response.value      | string         | Base64-encoded Protobuf response from the queried module |
| result.response.height     | string         | Block height at which the query was executed             |
| result.response.proof\_ops | object \| null | Merkle proof operations if `prove=true`                  |

## Use Cases

* Power user / framework access to module state without the REST gateway
* Light clients that need state with verifiable Merkle proofs
* Tooling that bypasses the REST API for performance reasons

## Error Handling

| Status Code            | Error Message     | Cause                                               |
| ---------------------- | ----------------- | --------------------------------------------------- |
| 404                    | Not Found         | Missing or invalid `<ACCESS-TOKEN>`                 |
| -32602                 | Invalid params    | Request parameters are missing or malformed         |
| -32603                 | Internal error    | Server-side error while processing the request      |
| 429                    | Too Many Requests | Rate limit exceeded for your plan                   |
| non-zero response.code | Query failed      | Invalid path, malformed query data, or module error |

## SDK Integration

{% tabs %}
{% tab title="CosmJS" %}
```javascript
import { Tendermint37Client, HttpClient } from '@cosmjs/tendermint-rpc';

const httpClient = new HttpClient('https://go.getblock.io/<ACCESS-TOKEN>/');
const client = await Tendermint37Client.create(httpClient);

// CosmJS exposes typed methods on the client (e.g. client.status(), client.block()).
// For raw access to any RPC method:
const result = await httpClient.execute({"jsonrpc": "2.0", "id": "getblock.io", "method": "abci_query", "params": {"path": "/cosmos.bank.v1beta1.Query/Balance", "data": "0a2d636f736d6f73317436726c676638306a6438", "prove": false}});
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
    'method': 'abci_query',
    'params': {"path": "/cosmos.bank.v1beta1.Query/Balance", "data": "0a2d636f736d6f73317436726c676638306a6438", "prove": false}
}})
print(response.json())
```
{% endtab %}
{% endtabs %}
