---
description: >-
  Example code for the getchaintips JSON-RPC method. Complete guide on how to
  use getchaintips JSON-RPC in GetBlock Web3 documentation.
---

# getchaintips - Bitcoin Cash

This method returns information about all known tips in the block tree, including the main chain and any orphaned branches. It is used to detect forks the node has seen.

## Parameters

This method does not accept any parameters.

## Request

{% tabs %}
{% tab title="cURL" %}
{% code overflow="wrap" %}
```bash
curl --location --request POST 'https://go.getblock.io/<ACCESS-TOKEN>/' \
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
const { data } = await axios.post('https://go.getblock.io/<ACCESS-TOKEN>/', {
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
    'https://go.getblock.io/<ACCESS-TOKEN>/',
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
        .post("https://go.getblock.io/<ACCESS-TOKEN>/")
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
            "branchlen": 0,
            "hash": "0000000000000000023a561e1ea370153aac5d1504726d1a039032831c05fcfc",
            "height": 684634,
            "status": "active"
        },
        {
            "branchlen": 1,
            "hash": "000000000000000001d3a318372df5d1eec54462a0d7471ae1cdf49838f793dd",
            "height": 683516,
            "status": "valid-headers"
        }
    ]
}
```

## Response Parameters

| Parameter | Type         | Description                                      |
| --------- | ------------ | ------------------------------------------------ |
| error     | null\|object | Error object when the call fails, otherwise null |
| id        | string       | Request identifier matching the request          |
| result    | array        | Array of chain tip objects, one per known branch |

### Tip Entry

| Field     | Type    | Description                                           |
| --------- | ------- | ----------------------------------------------------- |
| height    | numeric | Height of the branch tip                              |
| hash      | string  | Block hash of the branch tip                          |
| branchlen | numeric | Length of the branch, 0 for the main chain            |
| status    | string  | Status of the branch, such as active or valid-headers |

## Use Cases

* **Fork Monitoring**: Detect competing branches the node has observed
* **Reorg Alerts**: Notice when a non-active tip approaches the main chain length
* **Node Health**: Confirm the active tip matches the expected network height
* **Consensus Research**: Study orphan branch frequency over time

## Error Handling

| Error Code | Message         | Description                                   |
| ---------- | --------------- | --------------------------------------------- |
| -32700     | Parse error     | Request body is not valid JSON                |
| -32600     | Invalid request | The JSON sent is not a valid request object   |
| -32603     | Internal error  | Node failed to read the requested chain state |
