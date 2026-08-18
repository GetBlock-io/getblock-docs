---
description: >-
  Example code for the getblockhash JSON-RPC method. Сomplete guide on how to
  use the getblockhash JSON-RPC method in GetBlock.io Web3 documentation.
---

# getblockhash - Zcash

This method returns the block hash at a given height. Used by indexers iterating through historical blocks and by explorers resolving user-supplied height inputs to hashes.

## Parameters

| Parameter | Type    | Required | Description                 |
| --------- | ------- | -------- | --------------------------- |
| `height`  | integer | Yes      | Block height (0 or greater) |

## Request

{% tabs %}
{% tab title="cURL" %}
{% code overflow="wrap" %}
```bash
curl --location --request POST 'https://shared.eu-central-1.getblock.io/<ACCESS-TOKEN>/' \
--header 'Content-Type: application/json' \
--data-raw '{
    "jsonrpc": "2.0",
    "method": "getblockhash",
    "params": [
        2856342
    ],
    "id": "getblock.io"
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
    method: 'getblockhash',
    params: [
        2856342
    ],
    id: 'getblock.io'
}, {
    headers: { 'Content-Type': 'application/json' }
});

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
        'method': 'getblockhash',
        'params': [
            2856342
        ],
        'id': 'getblock.io'
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
        .header("Content-Type", "application/json")
        .json(&json!({
            "jsonrpc": "2.0",
            "method": "getblockhash",
            "params": [
                2856342
            ],
            "id": "getblock.io"
        }))
        .send()
        .await?
        .json::<Value>()
        .await?;
    
    println!("Result: {}", response["result"]);
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
    "result": "00000000018bb43cadf5b4c8a2e7f9d0e1a6b3c8d5e4f2a1b0c9d8e7f6a5b4c3"
}
```

## Response Parameters

| Parameter | Type   | Description                                            |
| --------- | ------ | ------------------------------------------------------ |
| `result`  | string | Hex-encoded 32-byte block hash at the requested height |

## Use Cases

* **Height-to-Hash Resolution**: Convert a user-supplied block height into a hash for subsequent detail queries
* **Historical Backfill**: Iterate heights 0..tip to enumerate the full chain by hash
* **Explorer Search**: Resolve numeric block searches to hashes for the detail view

## Error Handling

| Error Code | Message                   | Description                                    |
| ---------- | ------------------------- | ---------------------------------------------- |
| -8         | Block height out of range | Requested height exceeds the current chain tip |
| -32602     | Invalid params            | Height parameter is malformed or negative      |
| -32603     | Internal error            | Node failed to look up the block               |
