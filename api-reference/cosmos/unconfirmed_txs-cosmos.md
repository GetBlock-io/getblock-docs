---
description: >-
  Example code for the unconfirmed_txs JSON-RPC method. Complete guide on how to
  use unconfirmed_txs JSON-RPC in GetBlock Web3 documentation.
---

# unconfirmed\_txs - Cosmos

Returns transactions currently in the mempool. By default returns up to 30 — use `limit` to adjust. Useful for mempool monitoring and MEV analytics where allowed.

## Parameters

| Parameter | Type    | Required | Description                                  |
| --------- | ------- | -------- | -------------------------------------------- |
| limit     | integer | No       | Maximum transactions to return (default: 30) |

## Request Example

{% tabs %}
{% tab title="cURL" %}
```bash
# JSON-RPC over HTTP POST (canonical)
curl --location --request POST 'https://go.getblock.io/<ACCESS-TOKEN>/unconfirmed_txs' \
--header 'Content-Type: application/json' \
--data-raw '{
    "jsonrpc": "2.0",
    "method": "unconfirmed_txs",
    "params": {
        "limit": 30
    },
    "id": "getblock.io"
}'

# URI over HTTP GET (alternate, for simple methods)
curl --location --request GET 'https://go.getblock.io/<ACCESS-TOKEN>/unconfirmed_txs?limit=30'
```
{% endtab %}

{% tab title="JavaScript (Axios)" %}
```javascript
import axios from 'axios';

const data = JSON.stringify({
    "jsonrpc": "2.0",
    "method": "unconfirmed_txs",
    "params": {
        "limit": 30
    },
    "id": "getblock.io"
});

const config = {
    method: 'post',
    url: 'https://go.getblock.io/<ACCESS-TOKEN>/unconfirmed_txs',
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

url = "https://go.getblock.io/<ACCESS-TOKEN>/unconfirmed_txs"

payload = json.dumps({
    "jsonrpc": "2.0",
    "method": "unconfirmed_txs",
    "params": {
        "limit": 30
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
        "method": "unconfirmed_txs",
        "params": {
                "limit": 30
        },
        "id": "getblock.io"
});

    let response = client
        .post("https://go.getblock.io/<ACCESS-TOKEN>/unconfirmed_txs")
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
        "n_txs": "5",
        "total": "5",
        "total_bytes": "8420",
        "txs": [
            "Co0BCooBChwvY29zbW9zLmJhbmsudjFiZXRhMS5Nc2dTZW5kEmoKLWNvc21vczF0NnJsZ2Y4MGpkOHRhbnBubnZtbXhmcWRjbTljbjd3Y3RyczdoaxItY29zbW9zMWdoZDc1M3NoamVjajRpdjJ4Mmk4eHJrenphbnpqdnZjMjJ4MmtuGgoKBXVhdG9tEgExEnIKWg=="
        ]
    }
}
```
{% endcode %}

## Response Parameters

| Field               | Type            | Description                       |
| ------------------- | --------------- | --------------------------------- |
| result.n\_txs       | string          | Number of transactions returned   |
| result.total        | string          | Total transactions in the mempool |
| result.total\_bytes | string          | Total mempool size in bytes       |
| result.txs          | array of string | Base64-encoded transaction bytes  |

## Use Cases

* Mempool monitoring dashboards
* Validator-side fee market analysis
* Pending-transaction surveillance

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
const result = await httpClient.execute({"jsonrpc": "2.0", "id": "getblock.io", "method": "unconfirmed_txs", "params": {"limit": 30}});
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
    'method': 'unconfirmed_txs',
    'params': {"limit": 30}
}})
print(response.json())
```
{% endtab %}
{% endtabs %}
