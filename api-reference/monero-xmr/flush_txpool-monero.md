---
description: >-
  Example code for the flush_txpool JSON-RPC method. Complete guide on how to
  use flush_txpool JSON-RPC in GetBlock Web3 documentation.
---

# flush\_txpool - Monero

This method removes one or more transactions from the node's mempool. It is an administrative operation that affects shared infrastructure.

## Parameters

| Parameter | Type            | Required | Description                                                                |
| --------- | --------------- | -------- | -------------------------------------------------------------------------- |
| txids     | array of string | No       | Specific transaction hashes to remove. If omitted, flushes the entire pool |

## Request Example

{% tabs %}
{% tab title="cURL" %}
```bash
curl --location --request POST 'https://go.getblock.io/<ACCESS-TOKEN>/' \
--header 'Content-Type: application/json' \
--data-raw '{
    "jsonrpc": "2.0",
    "method": "flush_txpool",
    "params": {
        "txids": [
            "d6e48158472848e6687173a91ae6eebfa3e1d778e65252ee99d7515d63090408"
        ]
    },
    "id": "getblock.io"
}'
```
{% endtab %}

{% tab title="JavaScript (Axios)" %}
```javascript
import axios from 'axios';

const data = JSON.stringify({
    "jsonrpc": "2.0",
    "method": "flush_txpool",
    "params": {
        "txids": [
            "d6e48158472848e6687173a91ae6eebfa3e1d778e65252ee99d7515d63090408"
        ]
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
    "method": "flush_txpool",
    "params": {
        "txids": [
            "d6e48158472848e6687173a91ae6eebfa3e1d778e65252ee99d7515d63090408"
        ]
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
        "method": "flush_txpool",
        "params": {
                "txids": [
                        "d6e48158472848e6687173a91ae6eebfa3e1d778e65252ee99d7515d63090408"
                ]
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
    "id": "getblock.io",
    "jsonrpc": "2.0",
    "result": {
        "status": "OK"
    }
}
```
{% endcode %}

## Response Parameters

| Field         | Type   | Description                 |
| ------------- | ------ | --------------------------- |
| result.status | string | `OK` if the flush completed |

## Use Cases

* Local-node management during testing
* Removing specific stuck transactions on a dedicated node

## Error Handling

| Status Code | Error Message     | Cause                                       |
| ----------- | ----------------- | ------------------------------------------- |
| 404         | Not found         | Missing or invalid `<ACCESS-TOKEN>`         |
| -32602      | Invalid params    | Request parameters are missing or malformed |
| 429         | Too Many Requests | Rate limit exceeded for your plan           |
