---
description: >-
  Example code for the iota_tryGetPastObject JSON-RPC method. Complete guide on
  how to use iota_tryGetPastObject JSON-RPC method in GetBlock Web3
  documentation.
---

# iota\_tryGetPastObject - IOTA

Returns an object as it existed at a specific version, if the node still retains that historical version. Useful for auditing state changes.

## Parameters

| Parameter  | Type    | Required | Description             |
| ---------- | ------- | -------- | ----------------------- |
| object\_id | string  | Yes      | Object ID               |
| version    | integer | Yes      | Object version to fetch |
| options    | object  | Optional | Display options         |

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
    "method": "iota_tryGetPastObject",
    "params": ["0x5f2e...", 40, {"showContent": true}]
}'
```
{% endcode %}
{% endtab %}

{% tab title="Axios" %}
{% code title="example.js" %}
```javascript
const axios = require('axios');
const response = await axios.post('https://shared.eu-central-1.getblock.io/<ACCESS-TOKEN>/', { jsonrpc: '2.0', id: 'getblock.io', method: 'iota_tryGetPastObject', params: ["0x5f2e...", 40, {"showContent": true}] }, { headers: { 'Content-Type': 'application/json' } });
console.log(response.data.result);
```
{% endcode %}
{% endtab %}

{% tab title="Request" %}
{% code title="example.py" %}
```python
import requests
response = requests.post('https://shared.eu-central-1.getblock.io/<ACCESS-TOKEN>/', headers={'Content-Type': 'application/json'}, json={'jsonrpc': '2.0', 'id': 'getblock.io', 'method': 'iota_tryGetPastObject', 'params': ["0x5f2e...", 40, {"showContent": true}]})
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
    let res = client.post("https://shared.eu-central-1.getblock.io/<ACCESS-TOKEN>/").json(&json!({"jsonrpc":"2.0","id":"getblock.io","method":"iota_tryGetPastObject","params":["0x5f2e...", 40, {"showContent": true}]})).send().await?.json::<Value>().await?;
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
        "status": "VersionFound",
        "details": {
            "objectId": "0x5f2e...",
            "version": "40",
            "type": "0x2::coin::Coin<0x2::iota::IOTA>"
        }
    }
}
```

## Response Parameters

| Field          | Type   | Description                                                       |
| -------------- | ------ | ----------------------------------------------------------------- |
| jsonrpc        | string | JSON-RPC version ("2.0")                                          |
| id             | string | Request identifier ("getblock.io")                                |
| result.status  | string | VersionFound / ObjectNotExists / VersionNotFound / VersionTooHigh |
| result.details | object | The object at that version when found                             |

## Use Cases

* **Auditing**: Inspect an object at a past version
* **Debugging**: Compare versions of an object

## Error Handling

| Error                     | Message          | Description                                       |
| ------------------------- | ---------------- | ------------------------------------------------- |
| -32602                    | Invalid params   | A parameter was missing or malformed              |
| -32601                    | Method not found | The method name is misspelled or not served       |
| 403 / RBAC: access denied | Access denied    | The GetBlock access token is missing or incorrect |
