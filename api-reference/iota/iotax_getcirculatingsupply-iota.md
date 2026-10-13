---
description: >-
  Example code for the iotax_getCirculatingSupply JSON-RPC method. Complete
  guide on how to use iotax_getCirculatingSupply JSON-RPC method in GetBlock
  Web3 documentation.
---

# iotax\_getCirculatingSupply - IOTA

Returns a summary of the circulating supply of the native IOTA token.

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
    "method": "iotax_getCirculatingSupply",
    "params": []
}'
```
{% endcode %}
{% endtab %}

{% tab title="Axios" %}
{% code title="example.js" %}
```javascript
const axios = require('axios');
const response = await axios.post('https://shared.eu-central-1.getblock.io/<ACCESS-TOKEN>/', { jsonrpc: '2.0', id: 'getblock.io', method: 'iotax_getCirculatingSupply', params: [] }, { headers: { 'Content-Type': 'application/json' } });
console.log(response.data.result);
```
{% endcode %}
{% endtab %}

{% tab title="Request" %}
{% code title="example.py" %}
```python
import requests
response = requests.post('https://shared.eu-central-1.getblock.io/<ACCESS-TOKEN>/', headers={'Content-Type': 'application/json'}, json={'jsonrpc': '2.0', 'id': 'getblock.io', 'method': 'iotax_getCirculatingSupply', 'params': []})
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
    let res = client.post("https://shared.eu-central-1.getblock.io/<ACCESS-TOKEN>/").json(&json!({"jsonrpc":"2.0","id":"getblock.io","method":"iotax_getCirculatingSupply","params":[]})).send().await?.json::<Value>().await?;
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
        "circulatingSupply": "3700000000000000000",
        "totalSupply": "4600000000000000000",
        "atEpoch": "742"
    }
}
```

## Response Parameters

| Field                    | Type   | Description                        |
| ------------------------ | ------ | ---------------------------------- |
| jsonrpc                  | string | JSON-RPC version ("2.0")           |
| id                       | string | Request identifier ("getblock.io") |
| result.circulatingSupply | string | Circulating IOTA in base units     |
| result.totalSupply       | string | Total IOTA supply                  |
| result.atEpoch           | string | Epoch the figures apply to         |

## Use Cases

* **Market Data**: Report circulating supply
* **Dashboards**: Show supply metrics

## Error Handling

| Error                     | Message          | Description                                       |
| ------------------------- | ---------------- | ------------------------------------------------- |
| -32602                    | Invalid params   | A parameter was missing or malformed              |
| -32601                    | Method not found | The method name is misspelled or not served       |
| 403 / RBAC: access denied | Access denied    | The GetBlock access token is missing or incorrect |
