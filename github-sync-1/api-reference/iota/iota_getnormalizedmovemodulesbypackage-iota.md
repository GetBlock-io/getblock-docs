---
description: >-
  Example code for the iota_getNormalizedMoveModulesByPackage JSON-RPC method.
  Complete guide on how to use iota_getNormalizedMoveModulesByPackage JSON-RPC
  method in GetBlock Web3 documentation.
---

# iota\_getNormalizedMoveModulesByPackage - IOTA

Returns structured (normalized) representations of every Move module in a published package, including its structs and functions.

## Parameters

| Parameter | Type   | Required | Description       |
| --------- | ------ | -------- | ----------------- |
| package   | string | Yes      | Package object ID |

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
    "method": "iota_getNormalizedMoveModulesByPackage",
    "params": ["0x2"]
}'
```
{% endcode %}
{% endtab %}

{% tab title="Axios" %}
{% code title="example.js" %}
```javascript
const axios = require('axios');
const response = await axios.post('https://shared.eu-central-1.getblock.io/<ACCESS-TOKEN>/', { jsonrpc: '2.0', id: 'getblock.io', method: 'iota_getNormalizedMoveModulesByPackage', params: ["0x2"] }, { headers: { 'Content-Type': 'application/json' } });
console.log(response.data.result);
```
{% endcode %}
{% endtab %}

{% tab title="Request" %}
{% code title="example.py" %}
```python
import requests
response = requests.post('https://shared.eu-central-1.getblock.io/<ACCESS-TOKEN>/', headers={'Content-Type': 'application/json'}, json={'jsonrpc': '2.0', 'id': 'getblock.io', 'method': 'iota_getNormalizedMoveModulesByPackage', 'params': ["0x2"]})
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
    let res = client.post("https://shared.eu-central-1.getblock.io/<ACCESS-TOKEN>/").json(&json!({"jsonrpc":"2.0","id":"getblock.io","method":"iota_getNormalizedMoveModulesByPackage","params":["0x2"]})).send().await?.json::<Value>().await?;
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
    "result": {
        "coin": {
            "fileFormatVersion": 6,
            "address": "0x2",
            "name": "coin",
            "structs": {},
            "exposedFunctions": {}
        }
    }
}
```

## Response Parameters

| Field   | Type   | Description                             |
| ------- | ------ | --------------------------------------- |
| jsonrpc | string | JSON-RPC version ("2.0")                |
| id      | string | Request identifier ("getblock.io")      |
| result  | object | Map of module name to normalized module |

## Use Cases

* **Contract Introspection**: Discover a package's modules and ABIs
* **Tooling**: Generate typed bindings

## Error Handling

| Error                     | Message          | Description                                       |
| ------------------------- | ---------------- | ------------------------------------------------- |
| -32602                    | Invalid params   | A parameter was missing or malformed              |
| -32601                    | Method not found | The method name is misspelled or not served       |
| 403 / RBAC: access denied | Access denied    | The GetBlock access token is missing or incorrect |
