---
description: >-
  Example code for the iotax_getValidatorsApy JSON-RPC method. Complete guide on
  how to use iotax_getValidatorsApy JSON-RPC method in GetBlock Web3
  documentation.
---

# iotax\_getValidatorsApy - IOTA

Returns the annualized percentage yield (APY) for each validator, for comparing staking returns.

## Parameters

This method takes no parameters.

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
    "method": "iotax_getValidatorsApy",
    "params": []
}'
```
{% endcode %}
{% endtab %}

{% tab title="Axios" %}
{% code title="example.js" %}
```javascript
const axios = require('axios');
const response = await axios.post('https://shared.eu-central-1.getblock.io/<ACCESS-TOKEN>/', { jsonrpc: '2.0', id: 'getblock.io', method: 'iotax_getValidatorsApy', params: [] }, { headers: { 'Content-Type': 'application/json' } });
console.log(response.data.result);
```
{% endcode %}
{% endtab %}

{% tab title="Request" %}
{% code title="example.py" %}
```python
import requests
response = requests.post('https://shared.eu-central-1.getblock.io/<ACCESS-TOKEN>/', headers={'Content-Type': 'application/json'}, json={'jsonrpc': '2.0', 'id': 'getblock.io', 'method': 'iotax_getValidatorsApy', 'params': []})
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
    let res = client.post("https://shared.eu-central-1.getblock.io/<ACCESS-TOKEN>/").json(&json!({"jsonrpc":"2.0","id":"getblock.io","method":"iotax_getValidatorsApy","params":[]})).send().await?.json::<Value>().await?;
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
        "apys": [
            {
                "address": "0xval...",
                "apy": 0.0325
            },
            {
                "address": "0xval2...",
                "apy": 0.0298
            }
        ],
        "epoch": "742"
    }
}
```

## Response Parameters

| Field        | Type   | Description                        |
| ------------ | ------ | ---------------------------------- |
| jsonrpc      | string | JSON-RPC version ("2.0")           |
| id           | string | Request identifier ("getblock.io") |
| result.apys  | array  | Per-validator {address, apy}       |
| result.epoch | string | Epoch the APYs apply to            |

## Use Cases

* **Staking UX**: Rank validators by yield
* **Analytics**: Track validator performance

## Error Handling

| Error                     | Message          | Description                                       |
| ------------------------- | ---------------- | ------------------------------------------------- |
| -32602                    | Invalid params   | A parameter was missing or malformed              |
| -32601                    | Method not found | The method name is misspelled or not served       |
| 403 / RBAC: access denied | Access denied    | The GetBlock access token is missing or incorrect |
