---
description: >-
  Example code for the block JSON-RPC method. Complete guide on how to use block
  JSON-RPC in GetBlock Web3 documentation.
---

# block - Cronos

Returns the block at a given height, or the latest block when height is omitted, including its header, data (transactions), and commit signatures.

## Parameters

| Parameter | Type   | Required | Description                                         |
| --------- | ------ | -------- | --------------------------------------------------- |
| height    | string | Optional | Block height as a string; omit for the latest block |

## Request

{% tabs %}
{% tab title="cURL" %}
{% code overflow="wrap" %}
```bash
curl --location --request POST 'https://shared.eu-central-1.getblock.io/<ACCESS-TOKEN>/' \
--header 'Content-Type: application/json' \
--data-raw '{
    "jsonrpc": "2.0",
    "id": "getblock.io",
    "method": "block",
    "params": {
    "height": "12345678"
}
}'
```
{% endcode %}
{% endtab %}

{% tab title="Axios" %}
{% code title="example.js" %}
```javascript
const axios = require('axios');

const response = await axios.post('https://shared.eu-central-1.getblock.io/<ACCESS-TOKEN>/', {
    jsonrpc: '2.0',
    id: 'getblock.io',
    method: 'block',
    params: {
    "height": "12345678"
}
}, { headers: { 'Content-Type': 'application/json' } });

console.log(response.data.result);
```
{% endcode %}
{% endtab %}

{% tab title="Request" %}
{% code title="example.py" %}
```python
import requests

response = requests.post(
    'https://shared.eu-central-1.getblock.io/<ACCESS-TOKEN>/',
    headers={'Content-Type': 'application/json'},
    json={
        'jsonrpc': '2.0',
        'id': 'getblock.io',
        'method': 'block',
        'params': {
    "height": "12345678"
}
    }
)

print(response.json())
```
{% endcode %}
{% endtab %}

{% tab title="Rust" %}
{% code title="example.rs" %}
```rust
use reqwest::Client;
use serde_json::{json, Value};

#[tokio::main]
async fn main() -> Result<(), Box<dyn std::error::Error>> {
    let client = Client::new();
    let response = client
        .post("https://shared.eu-central-1.getblock.io/<ACCESS-TOKEN>/")
        .json(&json!({
            "jsonrpc": "2.0",
            "id": "getblock.io",
            "method": "block",
            "params": {
    "height": "12345678"
}
        }))
        .send().await?
        .json::<Value>().await?;
    println!("{}", response["result"]);
    Ok(())
}
```
{% endcode %}
{% endtab %}
{% endtabs %}

## Response

```json
{
    "jsonrpc": "2.0",
    "id": "getblock.io",
    "result": {
        "block_id": {
            "hash": "E1F2..."
        },
        "block": {
            "header": {
                "chain_id": "cronosmainnet_25-1",
                "height": "12345678",
                "time": "2025-11-01T12:00:00Z",
                "proposer_address": "F00D..."
            },
            "data": {
                "txs": [
                    "Cr0BC..."
                ]
            },
            "last_commit": {
                "height": "12345677",
                "round": 0
            }
        }
    }
}
```

## Response Fields

| Field          | Type   | Description                              |
| -------------- | ------ | ---------------------------------------- |
| block\_id      | object | Hash of the block                        |
| block.header   | object | Chain id, height, time, and proposer     |
| block.data.txs | array  | Base64-encoded transactions in the block |

## Use Cases

* **Block Inspection**: Read header fields for a height
* **Chain Following**: Poll the latest block by omitting height
* **Tx Extraction**: Decode base64 txs from block.data

## Error Handling

| Error                     | Message        | Description                                                 |
| ------------------------- | -------------- | ----------------------------------------------------------- |
| -32603 / Internal error   | Internal error | Height is above the current tip or below the retained range |
| 403 / RBAC: access denied | Access denied  | The GetBlock access token is missing or incorrect           |
