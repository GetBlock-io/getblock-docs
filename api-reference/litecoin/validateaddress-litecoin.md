---
description: >-
  Example code for the validateaddress JSON-RPC method. Complete guide on how to
  use validateaddress JSON-RPC in GetBlock Web3 documentation.
---

# validateaddress - Litecoin

Return information about the given litecoin address.

### Parameters

| Parameter | Type   | Description                       |
| --------- | ------ | --------------------------------- |
| address   | string | The litecoin address to validate. |

### Request

{% tabs %}
{% tab title="cURL" %}
{% code title="cURL" overflow="wrap" %}
```bash
curl --location --request POST 'https://go.getblock.io/<ACCESS-TOKEN>/' \
--header 'Content-Type: application/json' \
--data-raw '{
    "jsonrpc": "2.0",
    "method": "validateaddress",
    "params": ["LTC1address..."],
    "id": "getblock.io"
}'
```
{% endcode %}
{% endtab %}

{% tab title="Axios" %}
{% code title="Axios (JavaScript)" %}
```javascript
import axios from 'axios';

const data = JSON.stringify({
    "jsonrpc": "2.0",
    "method": "validateaddress",
    "params": ["LTC1address..."],
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
{% code title="Python (requests)" %}
```python
import requests
import json

url = "https://go.getblock.io/<ACCESS-TOKEN>/"

payload = json.dumps({
    "jsonrpc": "2.0",
    "method": "validateaddress",
    "params": ["LTC1address..."],
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
{% code title="Rust (reqwest)" %}
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
            "params": ["LTC1address..."],
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

{% code title="response.json" %}
```json
{
    "jsonrpc": "2.0",
    "id": "getblock.io",
    "result": {
        "isvalid": true,
        "address": "LTC1address...",
        "scriptPubKey": "76a914...",
        "isscript": false,
        "iswitness": true,
        "witness_version": 0,
        "witness_program": "..."
    }
}
```
{% endcode %}

### Response Parameters

| Field     | Type    | Description                          |
| --------- | ------- | ------------------------------------ |
| isvalid   | boolean | If the address is valid.             |
| address   | string  | The address validated.               |
| isscript  | boolean | If the address is a script address.  |
| iswitness | boolean | If the address is a witness address. |
|           |         |                                      |

### Use Cases

The `validateaddress` method is essential for:

* Address validation
* Payment verification
* Wallet functionality
* User input validation

### Error Handling

| Status Code | Error Message | Cause                            |
| ----------- | ------------- | -------------------------------- |
| 403         | Forbidden     | Missing or invalid ACCESS-TOKEN. |

### Integration Notes

The `validateaddress` method helps developers build robust applications that interact with the Litecoin blockchain.
