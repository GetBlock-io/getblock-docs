---
description: >-
  Example code for the validateaddress JSON-RPC method. Complete guide on how to
  use validateaddress JSON-RPC in GetBlock Web3 documentation.
---

# validateaddress - Dogecoin

This method validates a Dogecoin address and returns information about it.

## Parameters

| Parameter | Type   | Required | Description                       |
| --------- | ------ | -------- | --------------------------------- |
| address   | string | Yes      | The Dogecoin address to validate. |

## Request

{% tabs %}
{% tab title="cURL" %}
{% code title="cURL" %}
```bash
curl --location --request POST 'https://go.getblock.io/<ACCESS-TOKEN>/' \
--header 'Content-Type: application/json' \
--data-raw '{
    "jsonrpc": "2.0",
    "method": "validateaddress",
    "params": ["DLGbK6mCjBT67r8wjJqCg8hkFiBYV5JquH"],
    "id": "getblock.io"
}'
```
{% endcode %}
{% endtab %}

{% tab title="Axios" %}
{% code title="validateaddress.js" %}
```javascript
const axios = require('axios');

const data = JSON.stringify({
    "jsonrpc": "2.0",
    "method": "validateaddress",
    "params": ["DLGbK6mCjBT67r8wjJqCg8hkFiBYV5JquH"],
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
{% code title="validateaddress.py" %}
```python
import requests
import json

url = "https://go.getblock.io/<ACCESS-TOKEN>/"

payload = json.dumps({
    "jsonrpc": "2.0",
    "method": "validateaddress",
    "params": ["DLGbK6mCjBT67r8wjJqCg8hkFiBYV5JquH"],
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
{% code title="validateaddress.rs" %}
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
            "method": "validateaddress",
            "params": ["DLGbK6mCjBT67r8wjJqCg8hkFiBYV5JquH"],
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

{% code title="Response (JSON)" %}
```json
{
    "result": {
        "isvalid": true,
        "address": "DLGbK6mCjBT67r8wjJqCg8hkFiBYV5JquH",
        "scriptPubKey": "76a914a5f4d12ce3685781b227c1f39548ddef429e978388ac",
        "ismine": false,
        "iswatchonly": false,
        "isscript": false
    },
    "error": null,
    "id": "getblock.io"
}
```
{% endcode %}

## Response Parameters

| Field        | Type    | Description                                |
| ------------ | ------- | ------------------------------------------ |
| isvalid      | boolean | Whether the address is valid.              |
| address      | string  | The validated address.                     |
| scriptPubKey | string  | The hex-encoded scriptPubKey.              |
| ismine       | boolean | Whether the address belongs to the wallet. |
| iswatchonly  | boolean | Whether the address is watch-only.         |
| isscript     | boolean | Whether the address is a script address.   |

## Use Case

The `validateaddress` method is essential for:

* Validating user-provided addresses
* Payment form validation
* Address format verification
* Wallet integration
* Exchange deposit address verification

## Error Handling

| Error Code | Message   | Cause                            |
| ---------- | --------- | -------------------------------- |
| 403        | Forbidden | Missing or invalid ACCESS-TOKEN. |
