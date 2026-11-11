---
description: >-
  Example code for the chainSpec_v1_properties JSON-RPC method. Complete guide
  on how to use chainSpec_v1_properties JSON-RPC in GetBlock Web3 documentation.
---

# chainSpec\_v1\_properties - Kusama

Returns the chain properties (SS58 format, token decimals, token symbol) via the new JSON-RPC specification. Equivalent to `system_properties`.

{% hint style="info" %}
Available over both the JSON-RPC (HTTP) and WebSocket interfaces.
{% endhint %}

## Parameters

{% hint style="info" %}
This method takes no parameters; send an empty `params` array.
{% endhint %}

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
    "method": "chainSpec_v1_properties",
    "params": []
}'
```
{% endcode %}
{% endtab %}

{% tab title="Axios" %}
{% code title="example.js" %}
```javascript
const axios = require('axios');

const response = await axios.post('https://shared.eu-central-1.getblock.io/<ACCESS-TOKEN>/', {
    jsonrpc: '2.0',
    id: 'getblock.io',
    method: 'chainSpec_v1_properties',
    params: []
}, { headers: { 'Content-Type': 'application/json' } });

console.log(response.data.result);
```
{% endcode %}
{% endtab %}

{% tab title="Request" %}
{% code title="example.py" %}
```python
import requests

response = requests.post(
    'https://shared.eu-central-1.getblock.io/<ACCESS-TOKEN>/',
    headers={'Content-Type': 'application/json'},
    json={
        'jsonrpc': '2.0',
        'id': 'getblock.io',
        'method': 'chainSpec_v1_properties',
        'params': []
    }
)

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
    let response = client
        .post("https://shared.eu-central-1.getblock.io/<ACCESS-TOKEN>/")
        .json(&json!({
            "jsonrpc": "2.0",
            "id": "getblock.io",
            "method": "chainSpec_v1_properties",
            "params": []
        }))
        .send().await?
        .json::<Value>().await?;
    println!("{}", response["result"]);
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
        "ss58Format": 2,
        "tokenDecimals": 12,
        "tokenSymbol": "KSM"
    }
}
```

## Response Fields

| Field         | Type    | Description                |
| ------------- | ------- | -------------------------- |
| ss58Format    | integer | SS58 address prefix (2)    |
| tokenDecimals | integer | Native token decimals (12) |
| tokenSymbol   | string  | Native token symbol (KSM)  |

## Use Cases

* **Formatting**: Format balances and addresses
* **Modern Clients**: Read properties via the new spec
* **UI Setup**: Label amounts with the symbol

## Error Handling

| Error                     | Message        | Description                                       |
| ------------------------- | -------------- | ------------------------------------------------- |
| -32603 / Internal error   | Internal error | The node failed to return properties              |
| 403 / RBAC: access denied | Access denied  | The GetBlock access token is missing or incorrect |
