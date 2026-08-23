---
description: >-
  Example code for the verifymessage JSON-RPC method. Complete guide on how to
  use verifymessage JSON-RPC in GetBlock Web3 documentation.
---

# verifymessage - Bitcoin

This method verifies a signed message against a Bitcoin address, returning whether the signature is valid for the address and message.

## Parameters

| Parameter | Type   | Required | Description                         |
| --------- | ------ | -------- | ----------------------------------- |
| address   | string | Yes      | The address that signed the message |
| signature | string | Yes      | The base64-encoded signature        |
| message   | string | Yes      | The message that was signed         |

## Request

{% tabs %}
{% tab title="cURL" %}
{% code overflow="wrap" %}
```bash
curl --location --request POST 'https://shared.eu-central-1.getblock.io/<ACCESS-TOKEN>/' \
--header 'Content-Type: application/json' \
--data-raw '{
    "jsonrpc": "2.0",
    "method": "verifymessage",
    "params": ["1A1zP1eP5QGefi2DMPTfTL5SLmv7DivfNa", "H9L5yLFjti0QTHhPyFrZCT1V/MMnBtXKmoiKDZ78NDBjERki6ZTQZdSMCtkgoNmp17By8ktJfPuwhHYo1v7NkI=", "Hello, Bitcoin"],
    "id": "getblock.io"
}'
```
{% endcode %}
{% endtab %}

{% tab title="bitcoinjs-lib" %}
{% code title="example.js" %}
```javascript
const axios = require('axios');

const response = await axios.post('https://shared.eu-central-1.getblock.io/<ACCESS-TOKEN>/', {
    jsonrpc: '2.0',
    method: 'verifymessage',
    params: ["1A1zP1eP5QGefi2DMPTfTL5SLmv7DivfNa", "H9L5yLFjti0QTHhPyFrZCT1V/MMnBtXKmoiKDZ78NDBjERki6ZTQZdSMCtkgoNmp17By8ktJfPuwhHYo1v7NkI=", "Hello, Bitcoin"],
    id: 'getblock.io'
}, {
    headers: { 'Content-Type': 'application/json' }
});

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
        'method': 'verifymessage',
        'params': ["1A1zP1eP5QGefi2DMPTfTL5SLmv7DivfNa", "H9L5yLFjti0QTHhPyFrZCT1V/MMnBtXKmoiKDZ78NDBjERki6ZTQZdSMCtkgoNmp17By8ktJfPuwhHYo1v7NkI=", "Hello, Bitcoin"],
        'id': 'getblock.io'
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
        .header("Content-Type", "application/json")
        .json(&json!({
            "jsonrpc": "2.0",
            "method": "verifymessage",
            "params": ["1A1zP1eP5QGefi2DMPTfTL5SLmv7DivfNa", "H9L5yLFjti0QTHhPyFrZCT1V/MMnBtXKmoiKDZ78NDBjERki6ZTQZdSMCtkgoNmp17By8ktJfPuwhHYo1v7NkI=", "Hello, Bitcoin"],
            "id": "getblock.io"
        }))
        .send()
        .await?
        .json::<Value>()
        .await?;

    println!("Result: {}", response["result"]);
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
    "result": true
}
```

## Response Parameters

| Parameter | Type    | Description                                                |
| --------- | ------- | ---------------------------------------------------------- |
| jsonrpc   | string  | JSON-RPC protocol version ("2.0")                          |
| id        | string  | Request identifier matching the request                    |
| result    | boolean | true if the signature is valid for the address and message |

## Use Cases

* **Ownership Proofs**: Verify control of an address
* **Authentication**: Authenticate a user by signature
* **Message Integrity**: Confirm a message was signed by an address
* **Compliance**: Verify address ownership claims

## Error Handling

| Error Code | Message           | Description                                  |
| ---------- | ----------------- | -------------------------------------------- |
| 403        | Forbidden         | Missing or invalid ACCESS-TOKEN              |
| -8         | Invalid parameter | A parameter is out of range or malformed     |
| -32601     | Method not found  | The method is not available on this endpoint |
| -32602     | Invalid params    | A parameter is missing or has the wrong type |
