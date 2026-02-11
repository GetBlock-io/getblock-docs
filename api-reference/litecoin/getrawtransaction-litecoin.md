---
description: >-
  Example code for the getrawtransaction JSON-RPC method. Complete guide on how
  to use getrawtransaction JSON-RPC in GetBlock Web3 documentation.
---

# getrawtransaction - Litecoin

Returns the raw transaction data.

### Parameters

| Parameter | Type    | Description                                    |
| --------- | ------- | ---------------------------------------------- |
| txid      | string  | The transaction id.                            |
| verbose   | boolean | Optional. True for JSON object. Default=false. |

### Request examples

{% tabs %}
{% tab title="cURL" %}
{% code title="cURL" %}
```bash
curl --location --request POST 'https://go.getblock.io/<ACCESS-TOKEN>/' \
--header 'Content-Type: application/json' \
--data-raw '{
    "jsonrpc": "2.0",
    "method": "getrawtransaction",
    "params": ["txid123...", true],
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
    "method": "getrawtransaction",
    "params": ["txid123...", true],
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

{% tab title="Python (requests)" %}
{% code title="request.py" %}
```python
import requests
import json

url = "https://go.getblock.io/<ACCESS-TOKEN>/"

payload = json.dumps({
    "jsonrpc": "2.0",
    "method": "getrawtransaction",
    "params": ["txid123...", true],
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

{% tab title="Rust (reqwest)" %}
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
            "method": "getrawtransaction",
            "params": ["txid123...", true],
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
        "txid": "txid123...",
        "hash": "txid123...",
        "version": 2,
        "size": 225,
        "vsize": 144,
        "weight": 573,
        "locktime": 0,
        "vin": [],
        "vout": [],
        "hex": "0200000001..."
    }
}
```
{% endcode %}

### Response Parameters

| Field | Type   | Description                    |
| ----- | ------ | ------------------------------ |
| txid  | string | The transaction id.            |
| size  | number | The transaction size in bytes. |
| vin   | array  | Array of input objects.        |
| vout  | array  | Array of output objects.       |

### Use Case

The `getrawtransaction` method is essential for:

* Transaction inspection
* Block explorer functionality
* Transaction verification
* Debugging transactions

### Error Handling

| Status Code | Error Message | Cause                            |
| ----------- | ------------- | -------------------------------- |
| 403         | Forbidden     | Missing or invalid ACCESS-TOKEN. |

### Integration Notes

The `getrawtransaction` method helps developers build robust applications that interact with the Litecoin blockchain.
