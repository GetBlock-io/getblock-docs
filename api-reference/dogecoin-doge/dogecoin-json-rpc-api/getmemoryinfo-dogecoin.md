---
description: >-
  Example code for the getmemoryinfo JSON-RPC method. Complete guide on how to use
  getmemoryinfo JSON-RPC in GetBlock Web3 documentation.
---

# getmemoryinfo - Dogecoin

This method returns information about the node's memory usage.

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
    "method": "getmemoryinfo",
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
  jsonrpc: '2.0', method: 'getmemoryinfo', params: [], id: 'getblock.io'
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
        'method': 'getmemoryinfo',
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
            "method": "getmemoryinfo",
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
    "result": {
        "locked": {
            "used": 1856,
            "free": 63680,
            "total": 65536,
            "locked": 65536,
            "chunks_used": 58,
            "chunks_free": 3
        }
    }
}
```

## Response Parameters

| Parameter | Type         | Description                                      |
| --------- | ------------ | ------------------------------------------------ |
| error     | null\|object | Error object when the call fails, otherwise null |
| id        | string       | Request identifier matching the request          |
| result.locked.used | numeric | Bytes used in the locked memory pool |
| result.locked.free | numeric | Bytes available in the locked memory pool |
| result.locked.total | numeric | Total size of the locked memory pool |
| result.locked.locked | numeric | Bytes that could be locked into RAM |
| result.locked.chunks_used | numeric | Number of allocated chunks |
| result.locked.chunks_free | numeric | Number of unused chunks |

## Use Cases

* **Node Monitoring**: Track memory pressure on a node you operate
* **Diagnostics**: Include memory state in a support report

## Error Handling

| Error Code | Message | Description |
| ---------- | ------- | ----------- |
| -32700 | Parse error | Request body is not valid JSON |
| -32600 | Invalid request | The JSON sent is not a valid request object |
| -32603 | Internal error | Node failed to read the requested chain state |
