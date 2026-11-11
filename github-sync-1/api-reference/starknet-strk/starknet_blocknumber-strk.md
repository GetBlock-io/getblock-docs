---
description: >-
  Example code for the starknet_blockNumber JSON-RPC method. Complete guide on
  how to use starknet_blockNumber JSON-RPC in GetBlock Web3 documentation.
---

# starknet\_blockNumber - STRK

This method returns the number of the most recent block accepted on Starknet. It is the primary way to read the current chain tip.

## Parameters

* None

## Request

{% tabs %}
{% tab title="cURL" %}
{% code overflow="wrap" %}
```bash
curl --location --request POST 'https://shared.eu-central-1.getblock.io/<ACCESS-TOKEN>/' \
--header 'Content-Type: application/json' \
--data-raw '{
    "jsonrpc": "2.0",
    "method": "starknet_blockNumber",
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
    method: 'starknet_blockNumber',
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
        'method': 'starknet_blockNumber',
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
            "method": "starknet_blockNumber",
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
    "result": 700000
}
```

## Response Parameters

| Parameter | Type    | Description                             |
| --------- | ------- | --------------------------------------- |
| jsonrpc   | string  | JSON-RPC protocol version ("2.0")       |
| id        | string  | Request identifier matching the request |
| result    | integer | Number of the latest accepted block     |

## Use Cases

* **Chain Tip Polling**: Track new blocks for indexers
* **Confirmation Counting**: Compute confirmations as tip minus a transaction's block
* **Range Bounds**: Set the upper bound of an event query to the tip
* **Sync Checks**: Compare a local indexer height against the network tip

## Error Handling

| Error                     | Message          | Description                                       |
| ------------------------- | ---------------- | ------------------------------------------------- |
| 63 / UNEXPECTED\_ERROR    | Unexpected error | The node failed to return the latest block number |
| 403 / RBAC: access denied | Access denied    | The GetBlock access token is missing or incorrect |
