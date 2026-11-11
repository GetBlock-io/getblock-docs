---
description: >-
  Example code for the iotax_getCoinMetadata JSON-RPC method. Complete guide on
  how to use iotax_getCoinMetadata JSON-RPC method in GetBlock Web3
  documentation.
---

# iotax\_getCoinMetadata - IOTA

Returns the metadata (symbol, name, decimals, icon) for a coin type, used to display token amounts correctly.

## Parameters

| Parameter  | Type   | Required | Description               |
| ---------- | ------ | -------- | ------------------------- |
| coin\_type | string | Yes      | Fully-qualified coin type |

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
    "method": "iotax_getCoinMetadata",
    "params": ["0x2::iota::IOTA"]
}'
```
{% endcode %}
{% endtab %}

{% tab title="Axios" %}
{% code title="example.js" %}
```javascript
const axios = require('axios');
const response = await axios.post('https://shared.eu-central-1.getblock.io/<ACCESS-TOKEN>/', { jsonrpc: '2.0', id: 'getblock.io', method: 'iotax_getCoinMetadata', params: ["0x2::iota::IOTA"] }, { headers: { 'Content-Type': 'application/json' } });
console.log(response.data.result);
```
{% endcode %}
{% endtab %}

{% tab title="Request" %}
{% code title="example.py" %}
```python
import requests
response = requests.post('https://shared.eu-central-1.getblock.io/<ACCESS-TOKEN>/', headers={'Content-Type': 'application/json'}, json={'jsonrpc': '2.0', 'id': 'getblock.io', 'method': 'iotax_getCoinMetadata', 'params': ["0x2::iota::IOTA"]})
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
    let res = client.post("https://shared.eu-central-1.getblock.io/<ACCESS-TOKEN>/").json(&json!({"jsonrpc":"2.0","id":"getblock.io","method":"iotax_getCoinMetadata","params":["0x2::iota::IOTA"]})).send().await?.json::<Value>().await?;
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
        "decimals": 9,
        "name": "IOTA",
        "symbol": "IOTA",
        "description": "The native token of the IOTA network",
        "iconUrl": null,
        "id": "0x..."
    }
}
```

## Response Parameters

| Field           | Type    | Description                        |
| --------------- | ------- | ---------------------------------- |
| jsonrpc         | string  | JSON-RPC version ("2.0")           |
| id              | string  | Request identifier ("getblock.io") |
| result.decimals | integer | Decimal places for display         |
| result.symbol   | string  | Coin symbol                        |
| result.name     | string  | Coin name                          |

## Use Cases

* **Display**: Format token amounts with correct decimals
* **Token Lists**: Build a registry of coin metadata

## Error Handling

| Error                     | Message          | Description                                       |
| ------------------------- | ---------------- | ------------------------------------------------- |
| -32602                    | Invalid params   | A parameter was missing or malformed              |
| -32601                    | Method not found | The method name is misspelled or not served       |
| 403 / RBAC: access denied | Access denied    | The GetBlock access token is missing or incorrect |
