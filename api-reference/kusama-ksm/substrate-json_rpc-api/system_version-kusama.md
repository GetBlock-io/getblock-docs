---
description: >-
  Example code for the system_version JSON-RPC method. Complete guide on how to
  use system_version JSON-RPC in GetBlock Web3 documentation.
---

# system\_version - Kusama

Returns the version string of the node client implementation.

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
    "method": "system_version",
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
    method: 'system_version',
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
        'method': 'system_version',
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
            "method": "system_version",
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
    "result": "1.16.2-9c1e..."
}
```

## Response Fields

| Field  | Type   | Description           |
| ------ | ------ | --------------------- |
| result | string | Client version string |

## Use Cases

* **Version Tracking**: Report the node software version
* **Upgrade Coordination**: Confirm a fleet is on the expected version
* **Diagnostics**: Attach version to issue reports

## Error Handling

| Error                     | Message        | Description                                       |
| ------------------------- | -------------- | ------------------------------------------------- |
| -32603 / Internal error   | Internal error | The node failed to return the version             |
| 403 / RBAC: access denied | Access denied  | The GetBlock access token is missing or incorrect |
