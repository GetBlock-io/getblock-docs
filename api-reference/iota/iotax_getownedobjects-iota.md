---
description: >-
  Example code for the iotax_getOwnedObjects JSON-RPC method. Complete guide on
  how to use iotax_getOwnedObjects JSON-RPC method in GetBlock Web3
  documentation.
---

# iotax\_getOwnedObjects - IOTA

Returns the objects owned by an address, with an optional query filter (by type, struct, or owner) and display options, paginated.

## Parameters

| Parameter | Type    | Required | Description                                   |
| --------- | ------- | -------- | --------------------------------------------- |
| address   | string  | Yes      | Owner address                                 |
| query     | object  | Optional | {filter, options} to narrow and shape results |
| cursor    | string  | Optional | Paging cursor                                 |
| limit     | integer | Optional | Max objects to return                         |

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
    "method": "iotax_getOwnedObjects",
    "params": ["0xabc...", {"filter": {"StructType": "0x2::coin::Coin<0x2::iota::IOTA>"}, "options": {"showType": true}}, null, 50]
}'
```
{% endcode %}
{% endtab %}

{% tab title="Axios" %}
{% code title="example.js" %}
```javascript
const axios = require('axios');
const response = await axios.post('https://shared.eu-central-1.getblock.io/<ACCESS-TOKEN>/', { jsonrpc: '2.0', id: 'getblock.io', method: 'iotax_getOwnedObjects', params: ["0xabc...", {"filter": {"StructType": "0x2::coin::Coin<0x2::iota::IOTA>"}, "options": {"showType": true}}, null, 50] }, { headers: { 'Content-Type': 'application/json' } });
console.log(response.data.result);
```
{% endcode %}
{% endtab %}

{% tab title="Request" %}
{% code title="example.py" %}
```python
import requests
response = requests.post('https://shared.eu-central-1.getblock.io/<ACCESS-TOKEN>/', headers={'Content-Type': 'application/json'}, json={'jsonrpc': '2.0', 'id': 'getblock.io', 'method': 'iotax_getOwnedObjects', 'params': ["0xabc...", {"filter": {"StructType": "0x2::coin::Coin<0x2::iota::IOTA>"}, "options": {"showType": true}}, null, 50]})
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
    let res = client.post("https://shared.eu-central-1.getblock.io/<ACCESS-TOKEN>/").json(&json!({"jsonrpc":"2.0","id":"getblock.io","method":"iotax_getOwnedObjects","params":["0xabc...", {"filter": {"StructType": "0x2::coin::Coin<0x2::iota::IOTA>"}, "options": {"showType": true}}, null, 50]})).send().await?.json::<Value>().await?;
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
        "data": [
            {
                "data": {
                    "objectId": "0x9f...",
                    "version": "42",
                    "type": "0x2::coin::Coin<0x2::iota::IOTA>"
                }
            }
        ],
        "nextCursor": "0x9f...",
        "hasNextPage": true
    }
}
```

## Response Parameters

| Field             | Type   | Description                        |
| ----------------- | ------ | ---------------------------------- |
| jsonrpc           | string | JSON-RPC version ("2.0")           |
| id                | string | Request identifier ("getblock.io") |
| result.data       | array  | Owned objects matching the filter  |
| result.nextCursor | string | Cursor for the next page           |

## Use Cases

* **Wallet Inventory**: List a user's NFTs and coins
* **dApp State**: Find objects of a given type

## Error Handling

| Error                     | Message          | Description                                       |
| ------------------------- | ---------------- | ------------------------------------------------- |
| -32602                    | Invalid params   | A parameter was missing or malformed              |
| -32601                    | Method not found | The method name is misspelled or not served       |
| 403 / RBAC: access denied | Access denied    | The GetBlock access token is missing or incorrect |
