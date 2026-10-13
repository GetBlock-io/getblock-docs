---
description: >-
  Example code for the iotax_getDynamicFieldObject JSON-RPC method. Complete
  guide on how to use iotax_getDynamicFieldObject JSON-RPC method in GetBlock
  Web3 documentation.
---

# iotax\_getDynamicFieldObject - IOTA

Returns the full object stored at a specific dynamic field of a parent object, addressed by the field name.

## Parameters

| Parameter          | Type   | Required | Description                      |
| ------------------ | ------ | -------- | -------------------------------- |
| parent\_object\_id | string | Yes      | Parent object ID                 |
| name               | object | Yes      | Dynamic field name {type, value} |

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
    "method": "iotax_getDynamicFieldObject",
    "params": ["0x5f2e...", {"type": "u64", "value": "1"}]
}'
```
{% endcode %}
{% endtab %}

{% tab title="Axios" %}
{% code title="example.js" %}
```javascript
const axios = require('axios');
const response = await axios.post('https://shared.eu-central-1.getblock.io/<ACCESS-TOKEN>/', { jsonrpc: '2.0', id: 'getblock.io', method: 'iotax_getDynamicFieldObject', params: ["0x5f2e...", {"type": "u64", "value": "1"}] }, { headers: { 'Content-Type': 'application/json' } });
console.log(response.data.result);
```
{% endcode %}
{% endtab %}

{% tab title="Request" %}
{% code title="example.py" %}
```python
import requests
response = requests.post('https://shared.eu-central-1.getblock.io/<ACCESS-TOKEN>/', headers={'Content-Type': 'application/json'}, json={'jsonrpc': '2.0', 'id': 'getblock.io', 'method': 'iotax_getDynamicFieldObject', 'params': ["0x5f2e...", {"type": "u64", "value": "1"}]})
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
    let res = client.post("https://shared.eu-central-1.getblock.io/<ACCESS-TOKEN>/").json(&json!({"jsonrpc":"2.0","id":"getblock.io","method":"iotax_getDynamicFieldObject","params":["0x5f2e...", {"type": "u64", "value": "1"}]})).send().await?.json::<Value>().await?;
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
        "data": {
            "objectId": "0x77aa...",
            "version": "7",
            "type": "0x2::coin::Coin<0x2::iota::IOTA>",
            "content": {
                "dataType": "moveObject",
                "fields": {
                    "balance": "500000000"
                }
            }
        }
    }
}
```

## Response Parameters

| Field       | Type   | Description                            |
| ----------- | ------ | -------------------------------------- |
| jsonrpc     | string | JSON-RPC version ("2.0")               |
| id          | string | Request identifier ("getblock.io")     |
| result.data | object | The object stored at the dynamic field |

## Use Cases

* **Lookups**: Read one entry of an on-chain table by key

## Error Handling

| Error                     | Message          | Description                                       |
| ------------------------- | ---------------- | ------------------------------------------------- |
| -32602                    | Invalid params   | A parameter was missing or malformed              |
| -32601                    | Method not found | The method name is misspelled or not served       |
| 403 / RBAC: access denied | Access denied    | The GetBlock access token is missing or incorrect |
