---
description: >-
  Example code for the verifymessage JSON-RPC method. Complete guide on how to
  use verifymessage JSON-RPC in GetBlock Web3 documentation.
---

# verifymessage - Bitcoin

This method verifies a signed message.

### Parameters

| Parameter | Type   | Required | Description                                            |
| --------- | ------ | -------- | ------------------------------------------------------ |
| address   | string | Yes      | The Bitcoin address to use for signature verification. |
| signature | string | Yes      | The signature provided by the signer in base64.        |
| message   | string | Yes      | The message that was signed.                           |

### Request

{% tabs %}
{% tab title="cURL" %}
{% code overflow="wrap" %}
```bash
curl --location --request POST 'https://go.getblock.io/<ACCESS-TOKEN>/' \
--header 'Content-Type: application/json' \
--data-raw '{
    "jsonrpc": "2.0",
    "method": "verifymessage",
    "params": ["1D1ZrZNe3JUo7ZycKEYQQiQAWd9y54F4XX", "H+F77IaLJXmgS7OBrHr7zq7Wz7WCNmSLRVKr6Q...==", "Hello, World!"],
    "id": "getblock.io"
}'
```
{% endcode %}
{% endtab %}

{% tab title="Axios" %}
{% code overflow="wrap" %}
```javascript
import axios from 'axios';

const data = JSON.stringify({
    "jsonrpc": "2.0",
    "method": "verifymessage",
    "params": ["1D1ZrZNe3JUo7ZycKEYQQiQAWd9y54F4XX", "H+F77IaLJXmgS7OBrHr7zq7Wz7WCNmSLRVKr6Q...==", "Hello, World!"],
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
{% code overflow="wrap" %}
```python
import requests
import json

url = "https://go.getblock.io/<ACCESS-TOKEN>/"

payload = json.dumps({
    "jsonrpc": "2.0",
    "method": "verifymessage",
    "params": ["1D1ZrZNe3JUo7ZycKEYQQiQAWd9y54F4XX", "H+F77IaLJXmgS7OBrHr7zq7Wz7WCNmSLRVKr6Q...==", "Hello, World!"],
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
{% code overflow="wrap" %}
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
    "params": ["1D1ZrZNe3JUo7ZycKEYQQiQAWd9y54F4XX", "H+F77IaLJXmgS7OBrHr7zq7Wz7WCNmSLRVKr6Q...==", "Hello, World!"],
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

### Response

```json
{
    "jsonrpc": "2.0",
    "id": "getblock.io",
    "result": true
}
```

### Response Parameters

| Field  | Type    | Description                                      |
| ------ | ------- | ------------------------------------------------ |
| result | boolean | True if the signature is valid, false otherwise. |

### Use Case

The `verifymessage` method is essential for:

* Proving ownership of Bitcoin addresses
* Verifying signed agreements or messages
* Authentication systems using Bitcoin keys
* Proof of reserve verification
* Identity verification workflows
* Building signature verification services

### Error Handling

| Status Code | Error Message       | Cause                                        |
| ----------- | ------------------- | -------------------------------------------- |
| 403         | Forbidden           | Missing or invalid ACCESS-TOKEN.             |
| -3          | Invalid address     | The provided address is not valid.           |
| -5          | Malformed signature | The signature is not in valid base64 format. |

### Integration With Web3

The `verifymessage` method helps developers:

* Build proof-of-ownership systems
* Implement message authentication
* Create signature verification services
* Support identity verification
* Enable cryptographic proofs
