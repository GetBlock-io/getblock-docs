---
description: >-
  Example code for the decoderawtransaction JSON-RPC method. Complete guide on
  how to use decoderawtransaction JSON-RPC in GetBlock Web3 documentation.
---

# decoderawtransaction - Litecoin

Returns a JSON object representing the serialized, hex-encoded transaction.

### Parameters

| Parameter | Type   | Description                 |
| --------- | ------ | --------------------------- |
| hexstring | string | The transaction hex string. |

### Request

{% tabs %}
{% tab title="cURL" %}
{% code title="curl" %}
```bash
curl --location --request POST 'https://go.getblock.io/<ACCESS-TOKEN>/' \
--header 'Content-Type: application/json' \
--data-raw '{
    "jsonrpc": "2.0",
    "method": "decoderawtransaction",
    "params": ["0100000001..."],
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
    "method": "decoderawtransaction",
    "params": ["0100000001..."],
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
    "method": "decoderawtransaction",
    "params": ["0100000001..."],
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
            "method": "decoderawtransaction",
            "params": ["0100000001..."],
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
        "vout": []
    }
}
```
{% endcode %}

### Response Parameters

| Field   | Type   | Description              |
| ------- | ------ | ------------------------ |
| txid    | string | The transaction id.      |
| version | number | The version.             |
| vin     | array  | Array of input objects.  |
| vout    | array  | Array of output objects. |

### Use Case

The `decoderawtransaction` method is essential for:

* Transaction analysis
* Pre-broadcast verification
* Transaction debugging
* Educational tools

### Error Handling

| Status Code | Error Message | Cause                            |
| ----------- | ------------- | -------------------------------- |
| 403         | Forbidden     | Missing or invalid ACCESS-TOKEN. |

### Integration Notes

The `decoderawtransaction` method helps developers build robust applications that interact with the Litecoin blockchain.
