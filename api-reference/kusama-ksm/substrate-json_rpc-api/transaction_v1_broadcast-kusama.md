---
description: >-
  Example code for the transaction_v1_broadcast JSON-RPC method. Complete guide
  on how to use transaction_v1_broadcast JSON-RPC in GetBlock Web3
  documentation.
---

# transaction\_v1\_broadcast - Kusama

Submits a signed transaction for the node to broadcast and keep re-broadcasting, via the new JSON-RPC specification. Returns an operation id used to stop broadcasting with transaction\_v1\_stop. Fire-and-forget; it does not report inclusion (use transactionWatch\_v1\_submitAndWatch for status).

{% hint style="info" %}
Available over both the JSON-RPC (HTTP) and WebSocket interfaces.
{% endhint %}

## Parameters

| Parameter   | Type   | Required | Description                    |
| ----------- | ------ | -------- | ------------------------------ |
| transaction | string | Yes      | Hex-encoded signed transaction |

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
    "method": "transaction_v1_broadcast",
    "params": [
    "0x4d028400d43593c715fdd31c61141abd04a99fd6822c8558854ccde39a5684e7a56da27d..."
]
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
    method: 'transaction_v1_broadcast',
    params: [
    "0x4d028400d43593c715fdd31c61141abd04a99fd6822c8558854ccde39a5684e7a56da27d..."
]
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
        'method': 'transaction_v1_broadcast',
        'params': [
    "0x4d028400d43593c715fdd31c61141abd04a99fd6822c8558854ccde39a5684e7a56da27d..."
]
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
            "method": "transaction_v1_broadcast",
            "params": [
    "0x4d028400d43593c715fdd31c61141abd04a99fd6822c8558854ccde39a5684e7a56da27d..."
]
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
    "result": "operationId-a1b2c3"
}
```

## Response Fields

| Field  | Type   | Description |
| ------ | ------ | ----------- |
| result | string | null        |

## Use Cases

* **Reliable Broadcast**: Have the node re-broadcast until included
* **Modern Clients**: Use the new spec's broadcast call
* **Fire-and-Forget**: Submit without holding a subscription

## Error Handling

| Error                     | Message             | Description                                       |
| ------------------------- | ------------------- | ------------------------------------------------- |
| -32603 / Internal error   | Invalid transaction | The transaction could not be decoded              |
| 403 / RBAC: access denied | Access denied       | The GetBlock access token is missing or incorrect |
