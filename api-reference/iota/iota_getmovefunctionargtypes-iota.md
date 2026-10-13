---
description: >-
  Example code for the iota_getMoveFunctionArgTypes JSON-RPC method. Complete
  guide on how to use iota_getMoveFunctionArgTypes JSON-RPC method in GetBlock
  Web3 documentation.
---

# iota\_getMoveFunctionArgTypes - IOTA

Returns the argument types (object vs pure) of a Move function, which a client needs to correctly place arguments when building a call.

## Parameters

| Parameter | Type   | Required | Description       |
| --------- | ------ | -------- | ----------------- |
| package   | string | Yes      | Package object ID |
| module    | string | Yes      | Module name       |
| function  | string | Yes      | Function name     |

## Request

{% tabs %}
{% tab title="cURL" %}
{% code overflow="wrap" %}
```bash
curl --location --request POST 'https://shared.eu-central-1.getblock.io/<ACCESS-TOKEN>/' \
--header 'Content-Type: application/json' \
--data-raw '{
    "jsonrpc": "2.0",
    "id": "getblock.io",
    "method": "iota_getMoveFunctionArgTypes",
    "params": ["0x2", "coin", "balance"]
}'
```
{% endcode %}
{% endtab %}

{% tab title="Axios" %}
{% code title="example.js" %}
```javascript
const axios = require('axios');
const response = await axios.post('https://shared.eu-central-1.getblock.io/<ACCESS-TOKEN>/', { jsonrpc: '2.0', id: 'getblock.io', method: 'iota_getMoveFunctionArgTypes', params: ["0x2", "coin", "balance"] }, { headers: { 'Content-Type': 'application/json' } });
console.log(response.data.result);
```
{% endcode %}
{% endtab %}

{% tab title="Request" %}
{% code title="example.py" %}
```python
import requests
response = requests.post('https://shared.eu-central-1.getblock.io/<ACCESS-TOKEN>/', headers={'Content-Type': 'application/json'}, json={'jsonrpc': '2.0', 'id': 'getblock.io', 'method': 'iota_getMoveFunctionArgTypes', 'params': ["0x2", "coin", "balance"]})
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
    let res = client.post("https://shared.eu-central-1.getblock.io/<ACCESS-TOKEN>/").json(&json!({"jsonrpc":"2.0","id":"getblock.io","method":"iota_getMoveFunctionArgTypes","params":["0x2", "coin", "balance"]})).send().await?.json::<Value>().await?;
    println!("{}", res["result"]);
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
    "result": [
        "Object",
        "Pure"
    ]
}
```

## Response Parameters

| Field   | Type   | Description                                  |
| ------- | ------ | -------------------------------------------- |
| jsonrpc | string | JSON-RPC version ("2.0")                     |
| id      | string | Request identifier ("getblock.io")           |
| result  | array  | Per-argument classification (Object or Pure) |

## Use Cases

* **Call Building**: Classify arguments for a transaction

## Error Handling

| Error                     | Message          | Description                                       |
| ------------------------- | ---------------- | ------------------------------------------------- |
| -32602                    | Invalid params   | A parameter was missing or malformed              |
| -32601                    | Method not found | The method name is misspelled or not served       |
| 403 / RBAC: access denied | Access denied    | The GetBlock access token is missing or incorrect |
