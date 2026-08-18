---
description: >-
  Example code for the getbestblockheightandhash JSON-RPC method. Сomplete guide
  on how to use the getbestblockheightandhash JSON-RPC method in GetBlock.io
  Web3 documentation.
---

# getbestblockheightandhash - Zcash

This method returns the height and hash of the current chain tip together in a single call. A Zebra-specific extension that avoids the round-trip of calling `getblockcount` and `getbestblockhash` separately — useful for consistent tip snapshots in indexing pipelines.

## Parameters

This method takes no parameters.

## Request

{% tabs %}
{% tab title="cURL" %}
{% code overflow="wrap" %}
```bash
curl --location --request POST 'https://shared.eu-central-1.getblock.io/<ACCESS-TOKEN>/' \
--header 'Content-Type: application/json' \
--data-raw '{
    "jsonrpc": "2.0",
    "method": "getbestblockheightandhash",
    "params": [],
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
    method: 'getbestblockheightandhash',
    params: [],
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
        'method': 'getbestblockheightandhash',
        'params': [],
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
            "method": "getbestblockheightandhash",
            "params": [],
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

{% code overflow="wrap" %}
```json
{
    "jsonrpc": "2.0",
    "id": "getblock.io",
    "result": {
        "height": 3420499,
        "hash": [
            228,
            251,
            247,
            242,
            235,
            11,
            87,
            212,
            192,
            153,
            35,
            47,
            113,
            123,
            98,
            101,
            203,
            160,
            229,
            150,
            241,
            89,
            65,
            240,
            95,
            188,
            39,
            0,
            0,
            0,
            0,
            0
        ]
    }
}
```
{% endcode %}

## Response Parameters

| Parameter | Type    | Description                                     |
| --------- | ------- | ----------------------------------------------- |
| `height`  | integer | Height of the chain tip                         |
| `hash`    | string  | Hex-encoded 32-byte hash of the chain tip block |

## Use Cases

* **Consistent Tip Snapshots**: Retrieve height and hash atomically — critical when the chain may advance between calls
* **Indexer Anchor Points**: Anchor an indexing run to a specific tip position for later resumption
* **Reorg-Safe Progress Tracking**: Track chain progress with the height+hash pair to detect both advancement and reorgs

## Error Handling

| Error Code | Message          | Description                                                              |
| ---------- | ---------------- | ------------------------------------------------------------------------ |
| -32601     | Method not found | Endpoint runs a client that doesn't expose this Zebra-specific extension |
| -32603     | Internal error   | Node has no blocks in state                                              |
