---
description: >-
  Example code for the get_pool_tx_detail_info JSON-RPC method. Complete guide
  on how to use get_pool_tx_detail_info JSON-RPC in GetBlock Web3 documentation.
---

# get\_pool\_tx\_detail\_info - Nervos

Returns detailed status of a transaction currently in the pool — entry time, ancestors count and size, descendants count and size, score relative to ancestors and descendants.

## Parameters

| Parameter | Type | Required | Description      |
| --------- | ---- | -------- | ---------------- |
| tx\_hash  | H256 | Yes      | Transaction hash |

## Request Example

{% tabs %}
{% tab title="cURL" %}
```bash
curl --location --request POST 'https://go.getblock.io/<ACCESS-TOKEN>/' \
--header 'Content-Type: application/json' \
--data-raw '{
    "jsonrpc": "2.0",
    "method": "get_pool_tx_detail_info",
    "params": [
        "0xa0ef4eb5f4ceeb08a4c8524d84c5da95dce2f608e0ca2ec8091191b0f330c6e0"
    ],
    "id": "getblock.io"
}'
```
{% endtab %}

{% tab title="JavaScript (Axios)" %}
```javascript
import axios from 'axios';

const data = JSON.stringify({
    "jsonrpc": "2.0",
    "method": "get_pool_tx_detail_info",
    "params": [
        "0xa0ef4eb5f4ceeb08a4c8524d84c5da95dce2f608e0ca2ec8091191b0f330c6e0"
    ],
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
    "method": "get_pool_tx_detail_info",
    "params": [
        "0xa0ef4eb5f4ceeb08a4c8524d84c5da95dce2f608e0ca2ec8091191b0f330c6e0"
    ],
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
        "method": "get_pool_tx_detail_info",
        "params": [
                "0xa0ef4eb5f4ceeb08a4c8524d84c5da95dce2f608e0ca2ec8091191b0f330c6e0"
        ],
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
        "ancestors_count": "0x1",
        "ancestors_size": "0x115",
        "ancestors_fee": "0x3e8",
        "descendants_count": "0x1",
        "descendants_size": "0x115",
        "descendants_fee": "0x3e8",
        "entry_status": "pending",
        "pending_count": "0x42",
        "proposed_count": "0x12",
        "score_sortkey": "0x4e9d3a"
    }
}
```
{% endcode %}

## Response Parameters

| Field                     | Type   | Description                       |
| ------------------------- | ------ | --------------------------------- |
| result.entry\_status      | string | `pending` or `proposed`           |
| result.ancestors\_count   | Uint64 | Number of in-pool ancestors       |
| result.descendants\_count | Uint64 | Number of in-pool descendants     |
| result.ancestors\_size    | Uint64 | Total size of in-pool ancestors   |
| result.descendants\_size  | Uint64 | Total size of in-pool descendants |
| result.score\_sortkey     | Uint64 | Internal sortkey used by the pool |

## Use Cases

* Debugging stuck transactions
* RBF / fee bumping decision logic
* Mempool topology analysis

## Error Handling

| Status Code | Error Message     | Cause                                                |
| ----------- | ----------------- | ---------------------------------------------------- |
| 403         | Forbidden         | Missing or invalid `<ACCESS-TOKEN>`                  |
| -32602      | Invalid params    | Request parameters are missing or malformed          |
| -32601      | Method not found  | Method does not exist or is not enabled on this node |
| -32603      | Internal error    | Server-side error while processing the request       |
| 429         | Too Many Requests | Rate limit exceeded for your plan                    |

## SDK Integration

{% tabs %}
{% tab title="@ckb-lumos/lumos" %}
```javascript
import { RPC } from '@ckb-lumos/rpc';

const rpc = new RPC('https://go.getblock.io/<ACCESS-TOKEN>/');

// Lumos provides typed methods on `rpc` (e.g. rpc.getTipHeader(), rpc.getBlock(hash)).
// For raw access to any CKB RPC method:
const result = await rpc.send('get_pool_tx_detail_info', ["0xa0ef4eb5f4ceeb08a4c8524d84c5da95dce2f608e0ca2ec8091191b0f330c6e0"]);
console.log(result);
```
{% endtab %}

{% tab title="ckb-sdk-python / requests" %}
```python
# Using the official CKB SDK Python (ckb-sdk-python).
# For methods not yet wrapped, fall back to raw requests.
import requests

response = requests.post('https://go.getblock.io/<ACCESS-TOKEN>/', json={
    'jsonrpc': '2.0',
    'id': 'getblock.io',
    'method': 'get_pool_tx_detail_info',
    'params': ["0xa0ef4eb5f4ceeb08a4c8524d84c5da95dce2f608e0ca2ec8091191b0f330c6e0"]
})
print(response.json())
```
{% endtab %}
{% endtabs %}
