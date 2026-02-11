---
description: >-
  Example code for the getblocktemplate JSON-RPC method. Complete guide on how
  to use getblocktemplate JSON-RPC in GetBlock Web3 documentation.
---

# getblocktemplate - Litecoin

Returns data needed to construct a block to work on.

### Parameters

| Parameter         | Type   | Description                                 |
| ----------------- | ------ | ------------------------------------------- |
| template\_request | object | Optional. JSON object with optional fields. |

### Request

{% tabs %}
{% tab title="cURL" %}
{% code title="cURL" %}
```bash
curl --location --request POST 'https://go.getblock.io/<ACCESS-TOKEN>/' \
--header 'Content-Type: application/json' \
--data-raw '{
    "jsonrpc": "2.0",
    "method": "getblocktemplate",
    "params": [{}],
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
    "method": "getblocktemplate",
    "params": [{}],
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
    "method": "getblocktemplate",
    "params": [{}],
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
            "method": "getblocktemplate",
            "params": [{}],
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
        "capabilities": ["proposal"],
        "version": 536870912,
        "rules": ["csv", "segwit", "mweb"],
        "previousblockhash": "blockhash...",
        "transactions": [],
        "coinbaseaux": {},
        "coinbasevalue": 625000000,
        "target": "0000000000000...",
        "mintime": 1640000000,
        "mutable": ["time", "transactions", "prevblock"],
        "noncerange": "00000000ffffffff",
        "sigoplimit": 80000,
        "sizelimit": 4000000,
        "curtime": 1640000001,
        "bits": "1a0fffff",
        "height": 2750001
    }
}
```
{% endcode %}

### Response Parameters

| Field             | Type   | Description                            |
| ----------------- | ------ | -------------------------------------- |
| version           | number | The preferred block version.           |
| previousblockhash | string | The hash of current highest block.     |
| transactions      | array  | Contents of non-coinbase transactions. |
| coinbasevalue     | number | Maximum allowable input to coinbase.   |
| height            | number | The height of the next block.          |

### Use Case

The `getblocktemplate` method is essential for:

* Mining pool implementation
* Block construction
* Mining software development
* Solo mining

### Error Handling

| Status Code | Error Message | Cause                            |
| ----------- | ------------- | -------------------------------- |
| 403         | Forbidden     | Missing or invalid ACCESS-TOKEN. |

### Integration Notes

The `getblocktemplate` method helps developers build robust applications that interact with the Litecoin blockchain.
