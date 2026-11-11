---
description: >-
  Example code for the iotax_getAllBalances JSON-RPC method. Complete guide on
  how to use iotax_getAllBalances JSON-RPC method in GetBlock Web3
  documentation.
---

# iotax\_getAllBalances - IOTA

Returns the total balance for every coin type an address owns, in one call.

## Parameters

| Parameter | Type   | Required | Description   |
| --------- | ------ | -------- | ------------- |
| owner     | string | Yes      | Owner address |

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
    "method": "iotax_getAllBalances",
    "params": ["0xabc..."]
}'
```
{% endcode %}
{% endtab %}

{% tab title="Axios" %}
{% code title="example.js" %}
```javascript
const axios = require('axios');
const response = await axios.post('https://shared.eu-central-1.getblock.io/<ACCESS-TOKEN>/', { jsonrpc: '2.0', id: 'getblock.io', method: 'iotax_getAllBalances', params: ["0xabc..."] }, { headers: { 'Content-Type': 'application/json' } });
console.log(response.data.result);
```
{% endcode %}
{% endtab %}

{% tab title="Request" %}
{% code title="example.py" %}
```python
import requests
response = requests.post('https://shared.eu-central-1.getblock.io/<ACCESS-TOKEN>/', headers={'Content-Type': 'application/json'}, json={'jsonrpc': '2.0', 'id': 'getblock.io', 'method': 'iotax_getAllBalances', 'params': ["0xabc..."]})
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
    let res = client.post("https://shared.eu-central-1.getblock.io/<ACCESS-TOKEN>/").json(&json!({"jsonrpc":"2.0","id":"getblock.io","method":"iotax_getAllBalances","params":["0xabc..."]})).send().await?.json::<Value>().await?;
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
        {
            "coinType": "0x2::iota::IOTA",
            "coinObjectCount": 3,
            "totalBalance": "1500000000"
        },
        {
            "coinType": "0xabc::usdc::USDC",
            "coinObjectCount": 1,
            "totalBalance": "5000000"
        }
    ]
}
```

## Response Parameters

| Field   | Type   | Description                        |
| ------- | ------ | ---------------------------------- |
| jsonrpc | string | JSON-RPC version ("2.0")           |
| id      | string | Request identifier ("getblock.io") |
| result  | array  | One balance summary per coin type  |

## Use Cases

* **Portfolio**: Enumerate all token balances
* **Wallets**: Render a multi-asset balance view

## Error Handling

| Error                     | Message          | Description                                       |
| ------------------------- | ---------------- | ------------------------------------------------- |
| -32602                    | Invalid params   | A parameter was missing or malformed              |
| -32601                    | Method not found | The method name is misspelled or not served       |
| 403 / RBAC: access denied | Access denied    | The GetBlock access token is missing or incorrect |
