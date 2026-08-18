---
description: >-
  Example code for the getblockcount JSON-RPC method. Сomplete guide on how to
  use the getblockcount JSON-RPC method in GetBlock.io Web3 documentation.
---

# getblockcount - Zcash

This method returns the height of the current chain tip as an integer. The simplest chain-progress read — commonly polled by wallets, indexers, and monitoring dashboards.

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
    "method": "getblockcount",
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
    method: 'getblockcount',
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
        'method': 'getblockcount',
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
            "method": "getblockcount",
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

```json
{
    "jsonrpc": "2.0",
    "id": "getblock.io",
    "result": 3420501
}
```

## Response Parameters

| Parameter | Type    | Description                     |
| --------- | ------- | ------------------------------- |
| `result`  | integer | Height of the current chain tip |

## Use Cases

* **Chain Progress Polling**: Detect new blocks by comparing successive polls
* **Confirmation Counting**: Compute `getblockcount() - txBlockHeight` to display confirmation counts in a wallet UI
* **Sync Waiting**: Wait for the tip to reach a specific target height during backfills or migrations

## Error Handling

| Error Code | Message        | Description                 |
| ---------- | -------------- | --------------------------- |
| -32603     | Internal error | Node has no blocks in state |
