---
description: >-
  Example code for the blockchain JSON-RPC method. Complete guide on how to use
  blockchain JSON-RPC in GetBlock Web3 documentation.
---

# blockchain - Cosmos

Returns block metadata for a range of blocks, between `minHeight` and `maxHeight` inclusive. The response is capped at 20 blocks per call — paginate by adjusting `maxHeight` to get older ranges.

## Parameters

| Parameter | Type    | Required | Description                    |
| --------- | ------- | -------- | ------------------------------ |
| minHeight | integer | Yes      | Minimum block height (decimal) |
| maxHeight | integer | Yes      | Maximum block height (decimal) |

## Request Example

{% tabs %}
{% tab title="cURL" %}
```bash
# JSON-RPC over HTTP POST (canonical)
curl --location --request POST 'https://go.getblock.io/<ACCESS-TOKEN>/blockchain' \
--header 'Content-Type: application/json' \
--data-raw '{
    "jsonrpc": "2.0",
    "method": "blockchain",
    "params": {
        "minHeight": 23456785,
        "maxHeight": 23456789
    },
    "id": "getblock.io"
}'

# URI over HTTP GET (alternate, for simple methods)
curl --location --request GET 'https://go.getblock.io/<ACCESS-TOKEN>/blockchain?minHeight=23456785&maxHeight=23456789'
```
{% endtab %}

{% tab title="JavaScript (Axios)" %}
```javascript
import axios from 'axios';

const data = JSON.stringify({
    "jsonrpc": "2.0",
    "method": "blockchain",
    "params": {
        "minHeight": 23456785,
        "maxHeight": 23456789
    },
    "id": "getblock.io"
});

const config = {
    method: 'post',
    url: 'https://go.getblock.io/<ACCESS-TOKEN>/blockchain',
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

url = "https://go.getblock.io/<ACCESS-TOKEN>/blockchain"

payload = json.dumps({
    "jsonrpc": "2.0",
    "method": "blockchain",
    "params": {
        "minHeight": 23456785,
        "maxHeight": 23456789
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
        "method": "blockchain",
        "params": {
                "minHeight": 23456785,
                "maxHeight": 23456789
        },
        "id": "getblock.io"
});

    let response = client
        .post("https://go.getblock.io/<ACCESS-TOKEN>/blockchain")
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
        "last_height": "23456789",
        "block_metas": [
            {
                "block_id": {
                    "hash": "6E2A2A6FE7A5B4C2D4E6F8A0B2C4D6E8F0A2C4E6F8B0D2E4F6A8C0E2F4B6D8E0",
                    "parts": {
                        "total": 1,
                        "hash": "B5C8E4F1A6D3B9C2E5F8A1D4B7C0E3F6A9D2B5C8E1F4A7D0B3C6E9F2A5D8B1C4"
                    }
                },
                "block_size": "15234",
                "header": {
                    "chain_id": "cosmoshub-4",
                    "height": "23456789",
                    "time": "2026-05-26T13:42:18.5273Z"
                },
                "num_txs": "42"
            }
        ]
    }
}
```
{% endcode %}

## Response Parameters

| Field                                 | Type   | Description                                     |
| ------------------------------------- | ------ | ----------------------------------------------- |
| result.last\_height                   | string | Current chain tip height                        |
| result.block\_metas                   | array  | Array of block metadata entries, newest first   |
| result.block\_metas\[].block\_id.hash | string | Block hash                                      |
| result.block\_metas\[].block\_size    | string | Block size in bytes (decimal string)            |
| result.block\_metas\[].header.height  | string | Block height                                    |
| result.block\_metas\[].header.time    | string | ISO 8601 block timestamp                        |
| result.block\_metas\[].num\_txs       | string | Transaction count in the block (decimal string) |

## Use Cases

* Block-range scanning in indexers and analytics pipelines
* Building block list views in explorers
* Efficient bulk metadata retrieval before drilling into specific blocks

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
const result = await httpClient.execute({"jsonrpc": "2.0", "id": "getblock.io", "method": "blockchain", "params": {"minHeight": 23456785, "maxHeight": 23456789}});
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
    'method': 'blockchain',
    'params': {"minHeight": 23456785, "maxHeight": 23456789}
}})
print(response.json())
```
{% endtab %}
{% endtabs %}
