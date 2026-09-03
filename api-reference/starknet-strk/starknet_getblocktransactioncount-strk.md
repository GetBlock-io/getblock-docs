---
description: >-
  Example code for the starknet_getBlockTransactionCount JSON-RPC method.
  Complete guide on how to use starknet_getBlockTransactionCount JSON-RPC in
  GetBlock Web3 documentation.
---

# starknet\_getBlockTransactionCount - STRK

This method returns the number of transactions in the block identified by block\_id. It is used to size pagination or to sanity-check a block.

## Parameters

| Parameter | Type   | Required | Description |
| --------- | ------ | -------- | ----------- |
| block\_id | object | string   | Yes         |

## Request

{% tabs %}
{% tab title="cURL" %}
{% code overflow="wrap" %}
```bash
curl --location --request POST 'https://shared.eu-central-1.getblock.io/<ACCESS-TOKEN>/' \
--header 'Content-Type: application/json' \
--data-raw '{
    "jsonrpc": "2.0",
    "method": "starknet_getBlockTransactionCount",
    "params": ["latest"],
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
    method: 'starknet_getBlockTransactionCount',
    params: ['latest'],
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
        'method': 'starknet_getBlockTransactionCount',
        'params': ['latest'],
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
            "method": "starknet_getBlockTransactionCount",
            "params": ["latest"],
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
    "result": 2
}
```

## Response Parameters

| Parameter | Type    | Description                             |
| --------- | ------- | --------------------------------------- |
| jsonrpc   | string  | JSON-RPC protocol version ("2.0")       |
| id        | string  | Request identifier matching the request |
| result    | integer | Count of transactions in the block      |

## Use Cases

* **Pagination**: Bound index-based transaction iteration
* **Throughput Metrics**: Track transactions per block over time
* **Sanity Checks**: Confirm a block has the expected transaction count
* **Explorer Backends**: Show a transaction count on block pages

## Error Handling

| Error                     | Message         | Description                                       |
| ------------------------- | --------------- | ------------------------------------------------- |
| 24 / BLOCK\_NOT\_FOUND    | Block not found | No block matches the supplied block\_id           |
| 403 / RBAC: access denied | Access denied   | The GetBlock access token is missing or incorrect |
