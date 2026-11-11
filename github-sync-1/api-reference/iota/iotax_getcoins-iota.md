---
description: >-
  Example code for the iotax_getCoins JSON-RPC method. Complete guide on how to
  use iotax_getCoins JSON-RPC method in GetBlock Web3 documentation.
---

# iotax\_getCoins - IOTA

Returns an address's individual Coin objects for a given coin type, paginated — the inputs you select when building a payment.

## Parameters

| Parameter  | Type    | Required | Description              |
| ---------- | ------- | -------- | ------------------------ |
| owner      | string  | Yes      | Owner address            |
| coin\_type | string  | Optional | Coin type (default IOTA) |
| cursor     | string  | Optional | Paging cursor            |
| limit      | integer | Optional | Max coins to return      |

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
    "method": "iotax_getCoins",
    "params": ["0xabc...", null, null, 10]
}'
```
{% endcode %}
{% endtab %}

{% tab title="Axios" %}
{% code title="example.js" %}
```javascript
const axios = require('axios');
const response = await axios.post('https://shared.eu-central-1.getblock.io/<ACCESS-TOKEN>/', { jsonrpc: '2.0', id: 'getblock.io', method: 'iotax_getCoins', params: ["0xabc...", null, null, 10] }, { headers: { 'Content-Type': 'application/json' } });
console.log(response.data.result);
```
{% endcode %}
{% endtab %}

{% tab title="Request" %}
{% code title="example.py" %}
```python
import requests
response = requests.post('https://shared.eu-central-1.getblock.io/<ACCESS-TOKEN>/', headers={'Content-Type': 'application/json'}, json={'jsonrpc': '2.0', 'id': 'getblock.io', 'method': 'iotax_getCoins', 'params': ["0xabc...", null, null, 10]})
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
    let res = client.post("https://shared.eu-central-1.getblock.io/<ACCESS-TOKEN>/").json(&json!({"jsonrpc":"2.0","id":"getblock.io","method":"iotax_getCoins","params":["0xabc...", null, null, 10]})).send().await?.json::<Value>().await?;
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
                "coinType": "0x2::iota::IOTA",
                "coinObjectId": "0x9f...",
                "version": "42",
                "balance": "1000000000",
                "digest": "7Md..."
            }
        ],
        "nextCursor": "0x9f...",
        "hasNextPage": false
    }
}
```

## Response Parameters

| Field             | Type   | Description                                |
| ----------------- | ------ | ------------------------------------------ |
| jsonrpc           | string | JSON-RPC version ("2.0")                   |
| id                | string | Request identifier ("getblock.io")         |
| result.data       | array  | Coin objects with id, version, and balance |
| result.nextCursor | string | Cursor for the next page                   |

## Use Cases

* **Coin Selection**: Pick inputs for a payment
* **UTXO-style Accounting**: Enumerate coin objects

## Error Handling

| Error                     | Message          | Description                                       |
| ------------------------- | ---------------- | ------------------------------------------------- |
| -32602                    | Invalid params   | A parameter was missing or malformed              |
| -32601                    | Method not found | The method name is misspelled or not served       |
| 403 / RBAC: access denied | Access denied    | The GetBlock access token is missing or incorrect |
