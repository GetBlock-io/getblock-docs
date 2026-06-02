---
description: >-
  Example code for the block JSON-RPC method. Complete guide on how to use block
  JSON-RPC in GetBlock Web3 documentation.
---

# block - Cosmos

Returns the block at a specific height, including full block header, transactions, and validator commit. If `height` is omitted, returns the latest block.

## Parameters

| Parameter | Type    | Required | Description                                                     |
| --------- | ------- | -------- | --------------------------------------------------------------- |
| height    | integer | No       | Block height (decimal). Defaults to the latest block if omitted |

## Request Example

{% tabs %}
{% tab title="cURL" %}
```bash
# JSON-RPC over HTTP POST (canonical)
curl --location --request POST 'https://go.getblock.io/<ACCESS-TOKEN>/block' \
--header 'Content-Type: application/json' \
--data-raw '{
    "jsonrpc": "2.0",
    "method": "block",
    "params": {
        "height": 23456789
    },
    "id": "getblock.io"
}'

# URI over HTTP GET (alternate, for simple methods)
curl --location --request GET 'https://go.getblock.io/<ACCESS-TOKEN>/block?height=23456789'
```
{% endtab %}

{% tab title="JavaScript (Axios)" %}
```javascript
import axios from 'axios';

const data = JSON.stringify({
    "jsonrpc": "2.0",
    "method": "block",
    "params": {
        "height": 23456789
    },
    "id": "getblock.io"
});

const config = {
    method: 'post',
    url: 'https://go.getblock.io/<ACCESS-TOKEN>/block',
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

url = "https://go.getblock.io/<ACCESS-TOKEN>/block"

payload = json.dumps({
    "jsonrpc": "2.0",
    "method": "block",
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
        "method": "block",
        "params": {
                "height": 23456789
        },
        "id": "getblock.io"
    });

    let response = client
        .post("https://go.getblock.io/<ACCESS-TOKEN>/block")
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
                "hash": "B5C8E4F1A6D3B9C2E5F8A1D4B7C0E3F6A9D2B5C8E1F4A7D0B3C6E9F2A5D8B1C4"
            }
        },
        "block": {
            "header": {
                "version": {
                    "block": "11",
                    "app": "0"
                },
                "chain_id": "cosmoshub-4",
                "height": "23456789",
                "time": "2026-05-26T13:42:18.5273Z",
                "last_block_id": {
                    "hash": "A1B2C3D4E5F6...",
                    "parts": {
                        "total": 1,
                        "hash": "..."
                    }
                },
                "proposer_address": "8E2B7A8E6D4C2E0F1A3B5C7D9E1F2A4B6C8D0E2F"
            },
            "data": {
                "txs": [
                    "Co0BCooBChwvY29zbW9zLmJhbmsudjFiZXRhMS5Nc2dTZW5kEmoKLWNvc21vczF0NnJsZ2Y4MGpkOHRhbnBubnZtbXhmcWRjbTljbjd3Y3RyczdoaxItY29zbW9zMWdoZDc1M3NoamVjajRpdjJ4Mmk4eHJrenphbnpqdnZjMjJ4MmtuGgoKBXVhdG9tEgExEnIKWg=="
                ]
            },
            "evidence": {
                "evidence": []
            },
            "last_commit": {
                "height": "23456788",
                "round": 0,
                "block_id": {
                    "hash": "..."
                },
                "signatures": []
            }
        }
    }
}
```
{% endcode %}

## Response Parameters

| Field                                 | Type            | Description                                             |
| ------------------------------------- | --------------- | ------------------------------------------------------- |
| result.block\_id.hash                 | string          | Block hash (32 bytes hex)                               |
| result.block.header.chain\_id         | string          | Chain ID                                                |
| result.block.header.height            | string          | Block height                                            |
| result.block.header.time              | string          | ISO 8601 block timestamp                                |
| result.block.header.proposer\_address | string          | Address of the validator that proposed this block       |
| result.block.data.txs                 | array of string | Base64-encoded transaction bytes                        |
| result.block.last\_commit             | object          | Commit signatures from validators on the previous block |

## Use Cases

* Block explorers showing full block contents
* Indexers replaying historical blocks
* Validator monitoring (proposer rotation tracking)

## Error Handling

| Status Code | Error Message     | Cause                                          |
| ----------- | ----------------- | ---------------------------------------------- |
| 403         | Forbidden         | Missing or invalid `<ACCESS-TOKEN>`            |
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
const result = await httpClient.execute({"jsonrpc": "2.0", "id": "getblock.io", "method": "block", "params": {"height": 23456789}});
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
    'method': 'block',
    'params': {"height": 23456789}
})
print(response.json())
```
{% endtab %}
{% endtabs %}
