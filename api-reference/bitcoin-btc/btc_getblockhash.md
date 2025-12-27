---
description: >-
  Example code for the getblockhash JSON-RPC method. Complete guide on how to
  use getblockhash JSON-RPC in GetBlock Web3 documentation.
---

# getblockhash - Bitcoin

This method returns the hash of the block at the specified height in the best-block-chain.

### Parameters

| Parameter | Type   | Required | Description               |
| --------- | ------ | -------- | ------------------------- |
| height    | number | Yes      | The block height (index). |

### Request

{% tabs %}
{% tab title="cURL" %}
```bash
curl --location --request POST 'https://go.getblock.io/<ACCESS-TOKEN>/' \
--header 'Content-Type: application/json' \
--data-raw '{
    "jsonrpc": "2.0",
    "method": "getblockhash",
    "params": [820000],
    "id": "getblock.io"
}'
```
{% endtab %}

{% tab title="Axios" %}
```javascript
import axios from 'axios';

const data = JSON.stringify({
    "jsonrpc": "2.0",
    "method": "getblockhash",
    "params": [820000],
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
{% endtab %}

{% tab title="Request" %}
```python
import requests
import json

url = "https://go.getblock.io/<ACCESS-TOKEN>/"

payload = json.dumps({
    "jsonrpc": "2.0",
    "method": "getblockhash",
    "params": [820000],
    "id": "getblock.io"
})

headers = {
    'Content-Type': 'application/json'
}

response = requests.post(url, headers=headers, data=payload)
print(response.text)
```
{% endtab %}

{% tab title="Rust" %}
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
            "params": [820000],
            "id": "getblock.io"
        }"#)
        .send()
        .await?;
    
    println!("{}", response.text().await?);
    Ok(())
}
```
{% endtab %}
{% endtabs %}

### Response

```json
{
    "jsonrpc": "2.0",
    "id": "getblock.io",
    "result": "000000000000000000046b9302e08c16ea186950f42a5498320ddd1bd7ab3428"
}
```

### Response Parameters

| Field  | Type   | Description                     |
| ------ | ------ | ------------------------------- |
| result | string | The block hash as a hex string. |

### Use Case

The `getblockhash` method is essential for:

* Looking up blocks by height
* Building block explorer navigation
* Iterating through the blockchain
* Fetching specific historical blocks
* Implementing chain traversal algorithms
* Building height-based block references

### Error Handling

| Status Code | Error Message             | Cause                               |
| ----------- | ------------------------- | ----------------------------------- |
| 403         | Forbidden                 | Missing or invalid ACCESS-TOKEN.    |
| -8          | Block height out of range | The specified height doesn't exist. |

### Integration Notes

The `getblockhash` method helps developers:

* Navigate blockchain by height
* Build block explorers
* Implement chain scanning
* Create historical analysis tools
* Support height-based lookups
