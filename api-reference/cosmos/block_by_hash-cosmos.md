---
description: >-
  Example code for the block_by_hash JSON-RPC method. Complete guide on how to
  use block_by_hash JSON-RPC in GetBlock Web3 documentation.
---

# block\_by\_hash - Cosmos

Returns a block by its hash. Same response schema as `block`. Useful when you have a block hash from a transaction receipt or peer notification but don't know the height.

## Parameters

| Parameter | Type   | Required | Description                                                     |
| --------- | ------ | -------- | --------------------------------------------------------------- |
| hash      | string | Yes      | Block hash (hex-encoded, 32 bytes, with or without `0x` prefix) |

## Request Example

{% tabs %}
{% tab title="cURL" %}
```bash
# JSON-RPC over HTTP POST (canonical)
curl --location --request POST 'https://go.getblock.io/<ACCESS-TOKEN>/' \
--header 'Content-Type: application/json' \
--data-raw '{
    "jsonrpc": "2.0",
    "method": "block_by_hash",
    "params": {
        "hash": "6E2A2A6FE7A5B4C2D4E6F8A0B2C4D6E8F0A2C4E6F8B0D2E4F6A8C0E2F4B6D8E0"
    },
    "id": "getblock.io"
}'

# URI over HTTP GET (alternate, for simple methods)
curl --location --request GET 'https://go.getblock.io/<ACCESS-TOKEN>/block_by_hash?hash="6E2A2A6FE7A5B4C2D4E6F8A0B2C4D6E8F0A2C4E6F8B0D2E4F6A8C0E2F4B6D8E0"'
```
{% endtab %}

{% tab title="JavaScript (Axios)" %}
```javascript
import axios from 'axios';

const data = JSON.stringify({
    "jsonrpc": "2.0",
    "method": "block_by_hash",
    "params": {
        "hash": "6E2A2A6FE7A5B4C2D4E6F8A0B2C4D6E8F0A2C4E6F8B0D2E4F6A8C0E2F4B6D8E0"
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
    "method": "block_by_hash",
    "params": {
        "hash": "6E2A2A6FE7A5B4C2D4E6F8A0B2C4D6E8F0A2C4E6F8B0D2E4F6A8C0E2F4B6D8E0"
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
        "method": "block_by_hash",
        "params": {
            "hash": "6E2A2A6FE7A5B4C2D4E6F8A0B2C4D6E8F0A2C4E6F8B0D2E4F6A8C0E2F4B6D8E0"
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
        "block_id": {
            "hash": "6E2A2A6FE7A5B4C2D4E6F8A0B2C4D6E8F0A2C4E6F8B0D2E4F6A8C0E2F4B6D8E0",
            "parts": {
                "total": 1,
                "hash": "..."
            }
        },
        "block": {
            "header": {
                "chain_id": "cosmoshub-4",
                "height": "23456789",
                "time": "2026-05-26T13:42:18.5273Z"
            },
            "data": {
                "txs": []
            },
            "last_commit": {}
        }
    }
}
```
{% endcode %}

## Response Parameters

| Field                 | Type   | Description                              |
| --------------------- | ------ | ---------------------------------------- |
| result.block\_id.hash | string | Block hash (echoed)                      |
| result.block          | object | Full block data (same schema as `block`) |

## Use Cases

* Resolving a block from a known hash
* Indexers cross-referencing block hashes with their parent chains

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
const result = await httpClient.execute({
    "jsonrpc": "2.0",
    "id": "getblock.io",
    "method": "block_by_hash",
    "params": {
        "hash": "6E2A2A6FE7A5B4C2D4E6F8A0B2C4D6E8F0A2C4E6F8B0D2E4F6A8C0E2F4B6D8E0"
    }
});
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
    'method': 'block_by_hash',
    'params': {"hash": "6E2A2A6FE7A5B4C2D4E6F8A0B2C4D6E8F0A2C4E6F8B0D2E4F6A8C0E2F4B6D8E0"}
})
print(response.json())
```
{% endtab %}
{% endtabs %}
