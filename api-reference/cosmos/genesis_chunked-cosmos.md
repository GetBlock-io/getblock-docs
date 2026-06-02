---
description: >-
  Example code for the genesis_chunked JSON-RPC method. Complete guide on how to
  use genesis_chunked JSON-RPC in GetBlock Web3 documentation.
---

# genesis\_chunked - Cosmos

Returns a chunk of the genesis document. Chunks are 0-indexed; call repeatedly with increasing `chunk` values until you've fetched all chunks (the response includes a total chunk count).

## Parameters

| Parameter | Type    | Required | Description           |
| --------- | ------- | -------- | --------------------- |
| chunk     | integer | Yes      | Chunk index (0-based) |

## Request Example

{% tabs %}
{% tab title="cURL" %}
```bash
# JSON-RPC over HTTP POST (canonical)
curl --location --request POST 'https://go.getblock.io/<ACCESS-TOKEN>/genesis_chunked' \
--header 'Content-Type: application/json' \
--data-raw '{
    "jsonrpc": "2.0",
    "method": "genesis_chunked",
    "params": {
        "chunk": 0
    },
    "id": "getblock.io"
}'

# URI over HTTP GET (alternate, for simple methods)
curl --location --request GET 'https://go.getblock.io/<ACCESS-TOKEN>/genesis_chunked?chunk=0'
```
{% endtab %}

{% tab title="JavaScript (Axios)" %}
```javascript
import axios from 'axios';

const data = JSON.stringify({
    "jsonrpc": "2.0",
    "method": "genesis_chunked",
    "params": {
        "chunk": 0
    },
    "id": "getblock.io"
});

const config = {
    method: 'post',
    url: 'https://go.getblock.io/<ACCESS-TOKEN>/genesis_chunked',
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

url = "https://go.getblock.io/<ACCESS-TOKEN>/genesis_chunked"

payload = json.dumps({
    "jsonrpc": "2.0",
    "method": "genesis_chunked",
    "params": {
        "chunk": 0
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
        "method": "genesis_chunked",
        "params": {
                "chunk": 0
        },
        "id": "getblock.io"
});

    let response = client
        .post("https://go.getblock.io/<ACCESS-TOKEN>/genesis_chunked")
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
        "chunk": 0,
        "total": 47,
        "data": "ewogICJnZW5lc2lzX3RpbWUiOiAiMjAxOS0xMi0xMVQxNjoxMTozNFoiLAogICJjaGFpbl9pZCI6ICJjb3Ntb3NodWItNCIsCiAg..."
    }
}
```
{% endcode %}

## Response Parameters

| Field        | Type    | Description                                            |
| ------------ | ------- | ------------------------------------------------------ |
| result.chunk | integer | Index of this chunk (echoed)                           |
| result.total | integer | Total number of chunks composing the full genesis file |
| result.data  | string  | Base64-encoded chunk of the genesis JSON               |

## Use Cases

* Downloading the Cosmos Hub genesis file in pieces
* Reconstructing initial state for archival nodes
* Light client initialization from a trusted genesis

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

const httpClient = new HttpClient('https://go.getblock.io/<ACCESS-TOKEN>/genesis_chunked');
const client = await Tendermint37Client.create(httpClient);

// CosmJS exposes typed methods on the client (e.g. client.status(), client.block()).
// For raw access to any RPC method:
const result = await httpClient.execute({"jsonrpc": "2.0", "id": "getblock.io", "method": "genesis_chunked", "params": {"chunk": 0}});
console.log(result);
```
{% endtab %}

{% tab title="cosmpy / requests" %}
```python
# cosmpy wraps Cosmos REST / gRPC. For raw CometBFT RPC access, use requests.
import requests

response = requests.post('https://go.getblock.io/<ACCESS-TOKEN>/genesis_chunked', json={
    'jsonrpc': '2.0',
    'id': 'getblock.io',
    'method': 'genesis_chunked',
    'params': {"chunk": 0}
}})
print(response.json())
```
{% endtab %}
{% endtabs %}
