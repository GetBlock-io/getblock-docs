---
description: >-
  Example code for the iotax_getBalance JSON-RPC method. Complete guide on how
  to use iotax_getBalance JSON-RPC method in GetBlock Web3 documentation.
---

# iotax\_getBalance - IOTA

Returns an address's total balance for a single coin type (IOTA by default), summed across all its coin objects.

## Parameters

| Parameter  | Type   | Required | Description                                         |
| ---------- | ------ | -------- | --------------------------------------------------- |
| owner      | string | Yes      | Owner address                                       |
| coin\_type | string | Optional | Fully-qualified coin type (default 0x2::iota::IOTA) |

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
    "method": "iotax_getBalance",
    "params": ["0xabc...", null]
}'
```
{% endcode %}
{% endtab %}

{% tab title="Axios" %}
{% code title="example.js" %}
```javascript
const axios = require('axios');
const response = await axios.post('https://shared.eu-central-1.getblock.io/<ACCESS-TOKEN>/', { jsonrpc: '2.0', id: 'getblock.io', method: 'iotax_getBalance', params: ["0xabc...", null] }, { headers: { 'Content-Type': 'application/json' } });
console.log(response.data.result);
```
{% endcode %}
{% endtab %}

{% tab title="Request" %}
{% code title="example.py" %}
```python
import requests
response = requests.post('https://shared.eu-central-1.getblock.io/<ACCESS-TOKEN>/', headers={'Content-Type': 'application/json'}, json={'jsonrpc': '2.0', 'id': 'getblock.io', 'method': 'iotax_getBalance', 'params': ["0xabc...", null]})
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
    let res = client.post("https://shared.eu-central-1.getblock.io/<ACCESS-TOKEN>/").json(&json!({"jsonrpc":"2.0","id":"getblock.io","method":"iotax_getBalance","params":["0xabc...", null]})).send().await?.json::<Value>().await?;
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
        "coinType": "0x2::iota::IOTA",
        "coinObjectCount": 3,
        "totalBalance": "1500000000",
        "lockedBalance": {}
    }
}
```

## Response Parameters

| Field                  | Type    | Description                            |
| ---------------------- | ------- | -------------------------------------- |
| jsonrpc                | string  | JSON-RPC version ("2.0")               |
| id                     | string  | Request identifier ("getblock.io")     |
| result.coinType        | string  | The coin type queried                  |
| result.totalBalance    | string  | Total balance in the coin's base units |
| result.coinObjectCount | integer | Number of coin objects held            |

## Use Cases

* **Balance Reads**: Show a wallet's IOTA balance
* **Payment Checks**: Confirm funds before a transfer

## Error Handling

| Error                     | Message          | Description                                       |
| ------------------------- | ---------------- | ------------------------------------------------- |
| -32602                    | Invalid params   | A parameter was missing or malformed              |
| -32601                    | Method not found | The method name is misspelled or not served       |
| 403 / RBAC: access denied | Access denied    | The GetBlock access token is missing or incorrect |
