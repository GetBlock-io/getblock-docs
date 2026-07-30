---
description: >-
  Example code for the getblockhash JSON-RPC method. Complete guide on how to
  use getblockhash JSON-RPC in GetBlock Web3 documentation.
---

# getblockhash - Bitcoin Cash

This method returns the hash of the block at the given height in the best chain. It converts a height into the hash required by hash-based lookups.

## Parameters

| Parameter | Type    | Required | Description                                     |
| --------- | ------- | -------- | ----------------------------------------------- |
| height    | numeric | Yes      | The height index of the block in the best chain |

## Request

{% tabs %}
{% tab title="cURL" %}
{% code overflow="wrap" %}
```bash
curl --location --request POST 'https://go.getblock.io/<ACCESS-TOKEN>/' \
--header 'Content-Type: application/json' \
--data-raw '{
    "jsonrpc": "2.0",
    "method": "getblockhash",
    "params": [684634],
    "id": "getblock.io"
}'
```
{% endcode %}
{% endtab %}

{% tab title="bitcoinjs-lib" %}
{% code title="example.js" %}
```javascript
const axios = require('axios');
const { data } = await axios.post('https://go.getblock.io/<ACCESS-TOKEN>/', {
  jsonrpc: '2.0', method: 'getblockhash', params: [684634], id: 'getblock.io'
});
console.log(data.result);
```
{% endcode %}
{% endtab %}

{% tab title="Request" %}
{% code title="example.py" %}
```python
import requests

response = requests.post(
    'https://go.getblock.io/<ACCESS-TOKEN>/',
    headers={'Content-Type': 'application/json'},
    json={
        'jsonrpc': '2.0',
        'method': 'getblockhash',
        'params': [684634],
        'id': 'getblock.io'
    }
)

print(response.json()['result'])
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
        .post("https://go.getblock.io/<ACCESS-TOKEN>/")
        .header("Content-Type", "application/json")
        .json(&json!({
            "jsonrpc": "2.0",
            "method": "getblockhash",
            "params": [684634],
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
    "error": null,
    "id": "getblock.io",
    "result": "0000000000000000023a561e1ea370153aac5d1504726d1a039032831c05fcfc"
}
```

## Response Parameters

| Parameter | Type         | Description                                           |
| --------- | ------------ | ----------------------------------------------------- |
| error     | null\|object | Error object when the call fails, otherwise null      |
| id        | string       | Request identifier matching the request               |
| result    | string       | Hex-encoded hash of the block at the requested height |

## Use Cases

* **Height to Hash**: Resolve a known height into a hash for getblock or getblockheader
* **Sequential Indexing**: Walk the chain by height when building a block index
* **Checkpoint Verification**: Confirm a height maps to an expected block hash
* **Explorer Links**: Generate block detail links from height inputs

## Error Handling

| Error Code | Message                   | Description                                             |
| ---------- | ------------------------- | ------------------------------------------------------- |
| -8         | Block height out of range | The requested height is negative or above the chain tip |
| -1         | Invalid parameter type    | height is not an integer                                |
| -32603     | Internal error            | Node failed to read the block index                     |
