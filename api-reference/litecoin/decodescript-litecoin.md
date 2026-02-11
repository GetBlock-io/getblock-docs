---
description: >-
  Example code for the decodescript JSON-RPC method. Complete guide on how to
  use decodescript JSON-RPC in GetBlock Web3 documentation.
---

# decodescript - Litecoin

Decode a hex-encoded script.

### Parameters

| Parameter | Type   | Description             |
| --------- | ------ | ----------------------- |
| hexstring | string | The hex-encoded script. |

### Request

{% tabs %}
{% tab title="cURL" %}
{% code title="cURL" %}
```bash
curl --location --request POST 'https://go.getblock.io/<ACCESS-TOKEN>/' \
--header 'Content-Type: application/json' \
--data-raw '{
    "jsonrpc": "2.0",
    "method": "decodescript",
    "params": ["76a914..."],
    "id": "getblock.io"
}'
```
{% endcode %}
{% endtab %}

{% tab title="Axios (JavaScript)" %}
{% code title="axios.js" %}
```javascript
import axios from 'axios';

const data = JSON.stringify({
    "jsonrpc": "2.0",
    "method": "decodescript",
    "params": ["76a914..."],
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

{% tab title="Python" %}
{% code title="request.py" %}
```python
import requests
import json

url = "https://go.getblock.io/<ACCESS-TOKEN>/"

payload = json.dumps({
    "jsonrpc": "2.0",
    "method": "decodescript",
    "params": ["76a914..."],
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
{% code title="main.rs" %}
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
            "method": "decodescript",
            "params": ["76a914..."],
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
    "result": {
        "asm": "OP_DUP OP_HASH160 ... OP_EQUALVERIFY OP_CHECKSIG",
        "type": "pubkeyhash",
        "reqSigs": 1,
        "addresses": ["LTCaddress..."],
        "p2sh": "address..."
    }
}
```

### Response Parameters

| Field     | Type   | Description         |
| --------- | ------ | ------------------- |
| asm       | string | Script public key.  |
| type      | string | The output type.    |
| addresses | array  | Array of addresses. |

### Use Case

The `decodescript` method is essential for:

* Script analysis
* Address extraction
* Transaction debugging
* Smart contract analysis

### Error Handling

| Status Code | Error Message | Cause                            |
| ----------- | ------------- | -------------------------------- |
| 403         | Forbidden     | Missing or invalid ACCESS-TOKEN. |

### Integration Notes

The `decodescript` method helps developers build robust applications that interact with the Litecoin blockchain.
