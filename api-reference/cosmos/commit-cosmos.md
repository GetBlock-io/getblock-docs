---
description: >-
  Example code for the commit JSON-RPC method. Complete guide on how to use
  commit JSON-RPC in GetBlock Web3 documentation.
---

# commit - Cosmos

Returns the commit (validator signatures) for a block at a given height. The commit proves the validator set agreed on the block; light clients use this to verify chain progression.

## Parameters

| Parameter | Type    | Required | Description                                                    |
| --------- | ------- | -------- | -------------------------------------------------------------- |
| height    | integer | No       | Block height (decimal). Defaults to the latest committed block |

## Request Example

{% tabs %}
{% tab title="cURL" %}
```bash
# JSON-RPC over HTTP POST (canonical)
curl --location --request POST 'https://go.getblock.io/<ACCESS-TOKEN>/commit' \
--header 'Content-Type: application/json' \
--data-raw '{
    "jsonrpc": "2.0",
    "method": "commit",
    "params": {
        "height": 23456789
    },
    "id": "getblock.io"
}'

# URI over HTTP GET (alternate, for simple methods)
curl --location --request GET 'https://go.getblock.io/<ACCESS-TOKEN>/commit?height=23456789'
```
{% endtab %}

{% tab title="JavaScript (Axios)" %}
```javascript
import axios from 'axios';

const data = JSON.stringify({
    "jsonrpc": "2.0",
    "method": "commit",
    "params": {
        "height": 23456789
    },
    "id": "getblock.io"
});

const config = {
    method: 'post',
    url: 'https://go.getblock.io/<ACCESS-TOKEN>/commit',
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

url = "https://go.getblock.io/<ACCESS-TOKEN>/commit"

payload = json.dumps({
    "jsonrpc": "2.0",
    "method": "commit",
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
        "method": "commit",
        "params": {
                "height": 23456789
        },
        "id": "getblock.io"
});

    let response = client
        .post("https://go.getblock.io/<ACCESS-TOKEN>/commit")
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
        "signed_header": {
            "header": {
                "chain_id": "cosmoshub-4",
                "height": "23456789",
                "time": "2026-05-26T13:42:18.5273Z"
            },
            "commit": {
                "height": "23456789",
                "round": 0,
                "block_id": {
                    "hash": "6E2A2A6FE7A5B4C2D4E6F8A0B2C4D6E8F0A2C4E6F8B0D2E4F6A8C0E2F4B6D8E0",
                    "parts": {
                        "total": 1,
                        "hash": "..."
                    }
                },
                "signatures": [
                    {
                        "block_id_flag": 2,
                        "validator_address": "8E2B7A...",
                        "timestamp": "2026-05-26T13:42:18.5273Z",
                        "signature": "AAA..."
                    }
                ]
            }
        },
        "canonical": true
    }
}
```
{% endcode %}

## Response Parameters

| Field                                   | Type    | Description                              |
| --------------------------------------- | ------- | ---------------------------------------- |
| result.signed\_header.header            | object  | Block header                             |
| result.signed\_header.commit            | object  | Commit data with validator signatures    |
| result.signed\_header.commit.signatures | array   | Per-validator signatures on this block   |
| result.canonical                        | boolean | Whether this commit is canonical (final) |

## Use Cases

* Light client verification
* Cross-chain proofs (IBC relayers verify peer chain commits)
* Auditing validator participation in consensus

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

const httpClient = new HttpClient('https://go.getblock.io/<ACCESS-TOKEN>/commit');
const client = await Tendermint37Client.create(httpClient);

// CosmJS exposes typed methods on the client (e.g. client.status(), client.block()).
// For raw access to any RPC method:
const result = await httpClient.execute({"jsonrpc": "2.0", "id": "getblock.io", "method": "commit", "params": {"height": 23456789}});
console.log(result);
```
{% endtab %}

{% tab title="cosmpy / requests" %}
```python
# cosmpy wraps Cosmos REST / gRPC. For raw CometBFT RPC access, use requests.
import requests

response = requests.post('https://go.getblock.io/<ACCESS-TOKEN>/commit', json={
    'jsonrpc': '2.0',
    'id': 'getblock.io',
    'method': 'commit',
    'params': {"height": 23456789}
}})
print(response.json())
```
{% endtab %}
{% endtabs %}
