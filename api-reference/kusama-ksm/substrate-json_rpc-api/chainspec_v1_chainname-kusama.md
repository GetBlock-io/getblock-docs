---
description: >-
  Example code for the chainSpec_v1_chainName JSON-RPC method. Complete guide on
  how to use chainSpec_v1_chainName JSON-RPC in GetBlock Web3 documentation.
---

# chainSpec\_v1\_chainName - Kusama

Returns the chain name via the new JSON-RPC specification's chainSpec group. Equivalent to system\_chain, part of the modern, stable JSON-RPC API.

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
    "method": "chainSpec_v1_chainName",
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
    method: 'chainSpec_v1_chainName',
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
        'method': 'chainSpec_v1_chainName',
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
            "method": "chainSpec_v1_chainName",
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
    "result": "Kusama"
}
```

## Response Fields

| Field  | Type   | Description |
| ------ | ------ | ----------- |
| result | string | Chain name  |

## Use Cases

* **Modern Clients**: Use the new spec's chain identity call
* **Light Clients**: Confirm the chain in a spec-compliant client
* **Config**: Key config off the chain name

## Error Handling

| Error                     | Message        | Description                                       |
| ------------------------- | -------------- | ------------------------------------------------- |
| -32603 / Internal error   | Internal error | The node failed to return the chain name          |
| 403 / RBAC: access denied | Access denied  | The GetBlock access token is missing or incorrect |
