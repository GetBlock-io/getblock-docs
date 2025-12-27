---
description: >-
  Example code for the verifymessage JSON-RPC method. Complete guide on how to
  use verifymessage JSON-RPC in GetBlock Web3 documentation.
---

# verifymessage - Dogecoin

This method verifies a signed message.

{% hint style="info" %}
* Returns `true` if the signature is valid for the address and message.
* Returns `false` if the signature is invalid.
* Does not require the private key (public verification only).
* Compatible with signatures from `signmessage`.
{% endhint %}

## Parameters

| Parameter | Type   | Required | Description                                   |
| --------- | ------ | -------- | --------------------------------------------- |
| address   | string | Yes      | The Dogecoin address that signed the message. |
| signature | string | Yes      | The base64-encoded signature.                 |
| message   | string | Yes      | The original message that was signed.         |

## Request

{% tabs %}
{% tab title="cURL" %}
{% code title="cURL" overflow="wrap" %}
```bash
curl --location --request POST 'https://go.getblock.io/<ACCESS-TOKEN>/' \
--header 'Content-Type: application/json' \
--data-raw '{
    "jsonrpc": "2.0",
    "method": "verifymessage",
    "params": ["DLGbK6mCjBT67r8wjJqCg8hkFiBYV5JquH", "H1b2c3d4e5f6g7h8i9j0...", "Hello, Dogecoin!"],
    "id": "getblock.io"
}'
```
{% endcode %}
{% endtab %}

{% tab title="Axios" %}
{% code title="verifymessage.js" overflow="wrap" %}
```javascript
const axios = require('axios');

const data = JSON.stringify({
    "jsonrpc": "2.0",
    "method": "verifymessage",
    "params": ["DLGbK6mCjBT67r8wjJqCg8hkFiBYV5JquH", "H1b2c3d4e5f6g7h8i9j0...", "Hello, Dogecoin!"],
    "id": "getblock.io"
});

const config = {
    method: 'post',
    url: 'https://go.getblock.io/<ACCESS-TOKEN>/',
    headers: {
        'Content-Type': 'application/json'
    },
    data: data
};

axios(config)
    .then(response => console.log(JSON.stringify(response.data)))
    .catch(error => console.log(error));
```
{% endcode %}
{% endtab %}

{% tab title="Request" %}
{% code title="verifymessage.py" overflow="wrap" %}
```python
import requests
import json

url = "https://go.getblock.io/<ACCESS-TOKEN>/"

payload = json.dumps({
    "jsonrpc": "2.0",
    "method": "verifymessage",
    "params": ["DLGbK6mCjBT67r8wjJqCg8hkFiBYV5JquH", "H1b2c3d4e5f6g7h8i9j0...", "Hello, Dogecoin!"],
    "id": "getblock.io"
})

headers = {
    'Content-Type': 'application/json'
}

response = requests.post(url, headers=headers, data=payload)
print(response.text)
```
{% endcode %}
{% endtab %}

{% tab title="Rust" %}
{% code title="verifymessage.rs" overflow="wrap" %}
```rust
use reqwest::header;

#[tokio::main]
async fn main() -> Result<(), Box<dyn std::error::Error>> {
    let client = reqwest::Client::new();
    
    let response = client
        .post("https://go.getblock.io/<ACCESS-TOKEN>/")
        .header(header::CONTENT_TYPE, "application/json")
        .body(r#"{
            "jsonrpc": "2.0",
            "method": "verifymessage",
            "params": ["DLGbK6mCjBT67r8wjJqCg8hkFiBYV5JquH", "signature", "Hello, Dogecoin!"],
            "id": "getblock.io"
        }"#)
        .send()
        .await?;
    
    println!("{}", response.text().await?);
    Ok(())
}
```
{% endcode %}
{% endtab %}
{% endtabs %}

## Response

{% code title="response.json" %}
```json
{
    "result": true,
    "error": null,
    "id": "getblock.io"
}
```
{% endcode %}

## Response Parameters

| Field  | Type    | Description                                  |
| ------ | ------- | -------------------------------------------- |
| result | boolean | True if signature is valid, false otherwise. |
| error  | null    | Error object (null if no error).             |
| id     | string  | Request identifier.                          |

## Use Case

The `verifymessage` method is essential for:

* Verifying address ownership proofs
* Authentication validation
* Signature verification
* Trust verification systems
* Proof of identity checks

## Error Handling

| Error Code | Message             | Cause                            |
| ---------- | ------------------- | -------------------------------- |
| -3         | Invalid address     | Invalid Dogecoin address.        |
| -5         | Malformed signature | Invalid base64 signature.        |
| 403        | Forbidden           | Missing or invalid ACCESS-TOKEN. |

