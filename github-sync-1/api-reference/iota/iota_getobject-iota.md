---
description: >-
  Example code for the iota_getObject JSON-RPC method. Complete guide on how to
  use iota_getObject JSON-RPC method in GetBlock Web3 documentation.
---

# iota\_getObject - IOTA

Returns the current state of an object by its ID, with selectable fields (type, owner, content, bcs, previous transaction). Objects are IOTA's core state primitive.

## Parameters

| Parameter  | Type   | Required | Description                                                                         |
| ---------- | ------ | -------- | ----------------------------------------------------------------------------------- |
| object\_id | string | Yes      | Object ID (0x-prefixed)                                                             |
| options    | object | Optional | Display options: showType, showOwner, showContent, showBcs, showPreviousTransaction |

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
    "method": "iota_getObject",
    "params": ["0x5f2e...", {"showContent": true, "showOwner": true, "showType": true}]
}'
```
{% endcode %}
{% endtab %}

{% tab title="Axios" %}
{% code title="example.js" %}
```javascript
const axios = require('axios');
const response = await axios.post('https://shared.eu-central-1.getblock.io/<ACCESS-TOKEN>/', { jsonrpc: '2.0', id: 'getblock.io', method: 'iota_getObject', params: ["0x5f2e...", {"showContent": true, "showOwner": true, "showType": true}] }, { headers: { 'Content-Type': 'application/json' } });
console.log(response.data.result);
```
{% endcode %}
{% endtab %}

{% tab title="Request" %}
{% code title="example.py" %}
```python
import requests
response = requests.post('https://shared.eu-central-1.getblock.io/<ACCESS-TOKEN>/', headers={'Content-Type': 'application/json'}, json={'jsonrpc': '2.0', 'id': 'getblock.io', 'method': 'iota_getObject', 'params': ["0x5f2e...", {"showContent": true, "showOwner": true, "showType": true}]})
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
    let res = client.post("https://shared.eu-central-1.getblock.io/<ACCESS-TOKEN>/").json(&json!({"jsonrpc":"2.0","id":"getblock.io","method":"iota_getObject","params":["0x5f2e...", {"showContent": true, "showOwner": true, "showType": true}]})).send().await?.json::<Value>().await?;
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
            "objectId": "0x5f2e...",
            "version": "42",
            "digest": "9xK...",
            "type": "0x2::coin::Coin<0x2::iota::IOTA>",
            "owner": {
                "AddressOwner": "0xabc..."
            },
            "content": {
                "dataType": "moveObject",
                "fields": {
                    "balance": "1500000000"
                }
            }
        }
    }
}
```

## Response Parameters

| Field                | Type   | Description                                              |
| -------------------- | ------ | -------------------------------------------------------- |
| jsonrpc              | string | JSON-RPC version ("2.0")                                 |
| id                   | string | Request identifier ("getblock.io")                       |
| result.data.objectId | string | Object ID                                                |
| result.data.version  | string | Object version                                           |
| result.data.type     | string | Fully-qualified Move type                                |
| result.data.owner    | object | Ownership (AddressOwner, Shared, Immutable, ObjectOwner) |
| result.data.content  | object | Decoded Move fields when requested                       |

## Use Cases

* **State Reads**: Read any on-chain object
* **Wallets**: Inspect coins, NFTs, and dynamic fields

## Error Handling

| Error                     | Message          | Description                                       |
| ------------------------- | ---------------- | ------------------------------------------------- |
| -32602                    | Invalid params   | A parameter was missing or malformed              |
| -32601                    | Method not found | The method name is misspelled or not served       |
| 403 / RBAC: access denied | Access denied    | The GetBlock access token is missing or incorrect |
