---
description: >-
  Example code for the getchaintips JSON-RPC method. Complete guide on how to use
  getchaintips JSON-RPC in GetBlock Web3 documentation.
---

# getchaintips - Dogecoin

This method returns information about all known tips in the block tree, including the main chain and any orphaned branches.

## Parameters

This method does not accept any parameters.

## Request

{% tabs %}
{% tab title="cURL" %}
{% code overflow="wrap" %}
```bash
curl --location --request POST 'https://shared.eu-central-1.getblock.io/<ACCESS-TOKEN>/' \
--header 'Content-Type: application/json' \
--data-raw '{
    "jsonrpc": "2.0",
    "method": "getchaintips",
    "params": [],
    "id": "getblock.io"
}'
```
{% endcode %}
{% endtab %}

{% tab title="bitcoinjs-lib" %}
{% code title="example.js" %}
```javascript
const axios = require('axios');
const { data } = await axios.post('https://shared.eu-central-1.getblock.io/<ACCESS-TOKEN>/', {
  jsonrpc: '2.0', method: 'getchaintips', params: [], id: 'getblock.io'
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
    'https://shared.eu-central-1.getblock.io/<ACCESS-TOKEN>/',
    headers={'Content-Type': 'application/json'},
    json={
        'jsonrpc': '2.0',
        'method': 'getchaintips',
        'params': [],
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
        .post("https://shared.eu-central-1.getblock.io/<ACCESS-TOKEN>/")
        .header("Content-Type", "application/json")
        .json(&json!({
            "jsonrpc": "2.0",
            "method": "getchaintips",
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
    "error": null,
    "id": "getblock.io",
    "result": [
        {
            "height": 6378959,
            "hash": "7d6f509b16de07844787f97b50778d032eb026a470eb0e0fb5b42a73290e7848",
            "branchlen": 0,
            "status": "active"
        },
        {
            "height": 6378812,
            "hash": "9f0e7a1c4b2d8e3f5a6c9b0d1e2f3a4b5c6d7e8f9a0b1c2d3e4f5a6b7c8d9e0f",
            "branchlen": 1,
            "status": "valid-fork"
        }
    ]
}
```

## Response Parameters

| Parameter | Type         | Description                                      |
| --------- | ------------ | ------------------------------------------------ |
| error     | null\|object | Error object when the call fails, otherwise null |
| id        | string       | Request identifier matching the request          |
| result[].height | numeric | Height of the branch tip |
| result[].hash | string | Hash of the branch tip |
| result[].branchlen | numeric | Length of the branch. 0 for the main chain |
| result[].status | string | `active`, `valid-fork`, `valid-headers`, `headers-only`, or `invalid` |

## Use Cases

* **Reorg Monitoring**: Watch for competing branches near the tip
* **Fork Analysis**: Inspect how long an orphaned branch ran
* **Node Health**: Confirm exactly one tip reports `active`

## Error Handling

| Error Code | Message | Description |
| ---------- | ------- | ----------- |
| -32700 | Parse error | Request body is not valid JSON |
| -32600 | Invalid request | The JSON sent is not a valid request object |
| -32603 | Internal error | Node failed to read the requested chain state |
