---
description: >-
  Example code for the getblockhash JSON-RPC method. Complete guide on how to
  use getblockhash JSON-RPC in GetBlock Web3 documentation.
---

# getblockhash - Litecoin

This method returns hash of block in best-block-chain at height provided.

### Parameters

| Parameter | Type         | Description       |
| --------- | ------------ | ----------------- |
| height    | number (int) | The height index. |

### Request

{% tabs %}
{% tab title="cURL" %}
{% code title="cURL" %}
```bash
curl --location --request POST 'https://go.getblock.io/<ACCESS-TOKEN>/' \
--header 'Content-Type: application/json' \
--data-raw '{
    "jsonrpc": "2.0",
    "method": "getblockhash",
    "params": [2750000],
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
    "method": "getblockhash",
    "params": [2750000],
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
    "method": "getblockhash",
    "params": [2750000],
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
            "method": "getblockhash",
            "params": [2750000],
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
    "result": "2b27dd39d0d380791408561a3f11c45b191ce11f88940128702861409c902168",
    "error": null,
    "id": "getblock.io"
}
```

### Response Parameters

| Field  | Type   | Description                                                          |
| ------ | ------ | -------------------------------------------------------------------- |
| result | string | The hash of the block at the given height (64 character hex string). |

### Use Case

The `getblockhash` method is essential for:

* Converting block height to hash
* Sequential block scanning
* Historical data retrieval
* Block indexing operations
* Chain traversal by height

### Error Handling

| Status Code | Error Message             | Cause                                |
| ----------- | ------------------------- | ------------------------------------ |
| 403         | Forbidden                 | Missing or invalid ACCESS-TOKEN.     |
| -8          | Block height out of range | Height exceeds current chain height. |

### Integration Notes

The `getblockhash` method helps developers:

* Navigate the blockchain by height
* Build block height indexes
* Retrieve historical blocks
* Implement block scanners
* Create height-based queries
