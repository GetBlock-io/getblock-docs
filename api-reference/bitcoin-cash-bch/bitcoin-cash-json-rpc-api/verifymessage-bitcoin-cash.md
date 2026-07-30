---
description: >-
  Example code for the verifymessage JSON-RPC method. Complete guide on how to
  use the verifymessage JSON-RPC method in the GetBlock Web3 documentation.
---

# verifymessage - Bitcoin Cash

This method verifies that a signature was produced by the private key behind a given address for a specific message.

## Parameters

| Parameter | Type   | Required | Description                                        |
| --------- | ------ | -------- | -------------------------------------------------- |
| address   | string | Yes      | The address whose key allegedly signed the message |
| signature | string | Yes      | The base64 signature produced by the signer        |
| message   | string | Yes      | The message that was signed                        |

## Request

{% tabs %}
{% tab title="cURL" %}
{% code overflow="wrap" %}
```bash
curl --location --request POST 'https://go.getblock.io/<ACCESS-TOKEN>/' \
--header 'Content-Type: application/json' \
--data-raw '{
    "jsonrpc": "2.0",
    "method": "verifymessage",
    "params": ["bitcoincash:qpm2qsznhks23z7629mms6s4cwef74vcwvy22gdx6a", "H9L5yLFjti0QTHhPyFrZCT1V/MMnBtXKmoiKDZ78NDBjERki6ZTQZdSMCtkgoNmvhX0=", "Hello"],
    "id": "getblock.io"
}'
```
{% endcode %}
{% endtab %}

{% tab title="bitcoinjs-lib" %}
{% code title="example.js" %}
```javascript
const axios = require('axios');
const { data } = await axios.post('https://go.getblock.io/<ACCESS-TOKEN>/', {
  jsonrpc: '2.0', method: 'verifymessage',
  params: ['bitcoincash:qpm2qsznhks23z7629mms6s4cwef74vcwvy22gdx6a', signature, 'Hello'], id: 'getblock.io'
});
console.log(data.result);
```
{% endcode %}
{% endtab %}

{% tab title="Request" %}
{% code title="example.py" %}
```python
import requests

response = requests.post(
    'https://go.getblock.io/<ACCESS-TOKEN>/',
    headers={'Content-Type': 'application/json'},
    json={
        'jsonrpc': '2.0',
        'method': 'verifymessage',
        'params': ["bitcoincash:qpm2qsznhks23z7629mms6s4cwef74vcwvy22gdx6a", "H9L5yLFjti0QTHhPyFrZCT1V/MMnBtXKmoiKDZ78NDBjERki6ZTQZdSMCtkgoNmvhX0=", "Hello"],
        'id': 'getblock.io'
    }
)

print(response.json()['result'])
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
        .post("https://go.getblock.io/<ACCESS-TOKEN>/")
        .header("Content-Type", "application/json")
        .json(&json!({
            "jsonrpc": "2.0",
            "method": "verifymessage",
            "params": ["bitcoincash:qpm2qsznhks23z7629mms6s4cwef74vcwvy22gdx6a", "H9L5yLFjti0QTHhPyFrZCT1V/MMnBtXKmoiKDZ78NDBjERki6ZTQZdSMCtkgoNmvhX0=", "Hello"],
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
    "error": null,
    "id": "getblock.io",
    "result": true
}
```

## Response Parameters

| Parameter | Type         | Description                                                |
| --------- | ------------ | ---------------------------------------------------------- |
| error     | null\|object | Error object when the call fails, otherwise null           |
| id        | string       | Request identifier matching the request                    |
| result    | boolean      | true if the signature is valid for the address and message |

## Use Cases

* **Ownership Proofs**: Verify a user controls the key behind an address
* **Authentication**: Confirm signed challenges in a login flow
* **Message Integrity**: Check that a signed message was not altered
* **Dispute Evidence**: Validate a signature offered as proof of authorship

## Error Handling

| Error Code | Message           | Description                                  |
| ---------- | ----------------- | -------------------------------------------- |
| -22        | Decode failed     | The supplied data could not be decoded       |
| -8         | Invalid parameter | A required parameter is missing or malformed |
| -32603     | Internal error    | Node failed to process the request           |
