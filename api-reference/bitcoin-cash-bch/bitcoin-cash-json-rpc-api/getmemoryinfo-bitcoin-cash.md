---
description: >-
  Example code for the getmemoryinfo JSON-RPC method. Complete guide on how to
  use the getmemoryinfo JSON-RPC method in the GetBlock Web3 documentation.
---

# getmemoryinfo - Bitcoin Cash

This method returns information about the node's memory usage, either as a human-readable summary or as raw allocator statistics.

## Parameters

| Parameter | Type   | Required | Description                                                                    |
| --------- | ------ | -------- | ------------------------------------------------------------------------------ |
| mode      | string | No       | stats for a usage summary, or mallocinfo for raw allocator data. Default stats |

## Request

{% tabs %}
{% tab title="cURL" %}
{% code overflow="wrap" %}
```bash
curl --location --request POST 'https://go.getblock.io/<ACCESS-TOKEN>/' \
--header 'Content-Type: application/json' \
--data-raw '{
    "jsonrpc": "2.0",
    "method": "getmemoryinfo",
    "params": ["stats"],
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
  jsonrpc: '2.0', method: 'getmemoryinfo',
  params: ['stats'], id: 'getblock.io'
});
console.log(data.result.locked.used);
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
        'method': 'getmemoryinfo',
        'params': ["stats"],
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
            "method": "getmemoryinfo",
            "params": ["stats"],
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
    "result": {
        "locked": {
            "used": 130464,
            "free": 131072,
            "total": 261536,
            "locked": 261536,
            "chunks_used": 2038,
            "chunks_free": 2
        }
    }
}
```

## Response Parameters

| Parameter | Type         | Description                                      |
| --------- | ------------ | ------------------------------------------------ |
| error     | null\|object | Error object when the call fails, otherwise null |
| id        | string       | Request identifier matching the request          |
| result    | object       | Object of node memory usage information          |

### Locked Object

| Field | Type    | Description                             |
| ----- | ------- | --------------------------------------- |
| used  | numeric | Bytes of locked memory currently in use |
| free  | numeric | Bytes of locked memory available        |
| total | numeric | Total locked memory in bytes            |

## Use Cases

* **Node Diagnostics**: Monitor locked memory pools on a dedicated node
* **Capacity Alerts**: Alert when memory usage approaches configured limits
* **Performance Tuning**: Inspect allocator statistics for tuning
* **Operations Dashboards**: Track node memory alongside other health metrics

## Error Handling

| Error Code | Message         | Description                                   |
| ---------- | --------------- | --------------------------------------------- |
| -32700     | Parse error     | Request body is not valid JSON                |
| -32600     | Invalid request | The JSON sent is not a valid request object   |
| -32603     | Internal error  | Node failed to read the requested chain state |
