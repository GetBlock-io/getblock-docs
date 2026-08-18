---
description: >-
  Example code for the getbestblockhash JSON-RPC method. Сomplete guide on how
  to use the getbestblockhash JSON-RPC method in GetBlock.io Web3 documentation.
---

# getbestblockhash - Zcash

This method returns the hash of the current chain tip — the most recent block accepted into the canonical chain. Lightweight alternative to `getblockchaininfo` when only the tip hash is needed.

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
    "method": "getbestblockhash",
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
    method: 'getbestblockhash',
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
        'method': 'getbestblockhash',
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
            "method": "getbestblockhash",
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
    "result": "00000000015aa1af4cc4e77fd576ff5b011a9ab42170bbcc008b2b9a84648165"
}
```
{% endcode %}

## Response Parameters

| Parameter | Type   | Description                                     |
| --------- | ------ | ----------------------------------------------- |
| `result`  | string | Hex-encoded 32-byte hash of the chain tip block |

## Use Cases

* **Reorg Detection**: Poll the tip hash and compare to a stored previous value — a change without height advancement indicates a reorg
* **Cache Invalidation Key**: Use the tip hash as a version key for cached chain-derived data
* **Lightweight Progress Signal**: Cheap poll to detect chain advancement without loading full block metadata

## Error Handling

| Error Code | Message        | Description                                                           |
| ---------- | -------------- | --------------------------------------------------------------------- |
| -32603     | Internal error | Node has no blocks in state (fresh install) or failed to read the tip |
