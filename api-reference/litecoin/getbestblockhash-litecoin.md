---
description: >-
  Example code for the getbestblockhash JSON-RPC method. Complete guide on how
  to use getbestblockhash JSON-RPC in GetBlock Web3 documentation.
---

# getbestblockhash - Litecoin

This method returns the hash of the best (tip) block in the longest blockchain.

### Parameters

* None

### Request

{% tabs %}
{% tab title="cURL" %}
{% code title="cURL" %}
```bash
curl --location --request POST 'https://go.getblock.io/<ACCESS-TOKEN>/' \
--header 'Content-Type: application/json' \
--data-raw '{
    "jsonrpc": "2.0",
    "method": "getbestblockhash",
    "params": [],
    "id": "getblock.io"
}'
```
{% endcode %}
{% endtab %}

{% tab title="JavaScript (Axios)" %}
{% code title="axios.js" %}
```javascript
import axios from 'axios';

const data = JSON.stringify({
    "jsonrpc": "2.0",
    "method": "getbestblockhash",
    "params": [],
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
    "method": "getbestblockhash",
    "params": [],
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
            "method": "getbestblockhash",
            "params": [],
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

{% code overflow="wrap" %}
```json
{
    "result": "1b2a8a574ade97833ae8a7361d632fa02d44a6a2546076b4d5afced07f6c2137",
    "error": null,
    "id": "getblock.io"
}
```
{% endcode %}

### Response Parameters

| Field  | Type   | Description                                                        |
| ------ | ------ | ------------------------------------------------------------------ |
| result | string | The hash of the best block in the chain (64 character hex string). |

### Use Case

The `getbestblockhash` method is essential for:

* Getting the current chain tip
* Starting point for block traversal
* Chain reorganization detection
* Monitoring new blocks
* Synchronization verification

### Error Handling

| Status Code | Error Message | Cause                            |
| ----------- | ------------- | -------------------------------- |
| 403         | Forbidden     | Missing or invalid ACCESS-TOKEN. |

### Integration Notes

The `getbestblockhash` method helps developers:

* Track the current chain tip
* Detect new blocks
* Build block notification systems
* Verify chain consistency
* Start blockchain traversal
