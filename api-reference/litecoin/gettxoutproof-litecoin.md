---
description: >-
  Example code for the gettxoutproof JSON-RPC method. Complete guide on how to
  use gettxoutproof JSON-RPC in GetBlock Web3 documentation.
---

# gettxoutproof - Litecoin

Returns a hex-encoded proof that txid was included in a block.

### Parameters

| Parameter | Type   | Description                                                         |
| --------- | ------ | ------------------------------------------------------------------- |
| txids     | array  | Array of txids to filter.                                           |
| blockhash | string | Optional. If specified, looks for txid in the block with this hash. |

### Request

{% tabs %}
{% tab title="cURL" %}
```bash
curl --location --request POST 'https://go.getblock.io/<ACCESS-TOKEN>/' \
--header 'Content-Type: application/json' \
--data-raw '{
    "jsonrpc": "2.0",
    "method": "gettxoutproof",
    "params": [["txid1", "txid2"]],
    "id": "getblock.io"
}'
```
{% endtab %}

{% tab title="Axios (JavaScript)" %}
```javascript
import axios from 'axios';

const data = JSON.stringify({
    "jsonrpc": "2.0",
    "method": "gettxoutproof",
    "params": [["txid1", "txid2"]],
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

{% tab title="Python (requests)" %}
```python
import requests
import json

url = "https://go.getblock.io/<ACCESS-TOKEN>/"

payload = json.dumps({
    "jsonrpc": "2.0",
    "method": "gettxoutproof",
    "params": [["txid1", "txid2"]],
    "id": "getblock.io"
})

headers = {
    'Content-Type': 'application/json'
}

response = requests.post(url, headers=headers, data=payload)
print(response.text)
```
{% endtab %}

{% tab title="Rust (reqwest)" %}
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
            "method": "gettxoutproof",
            "params": [["txid1", "txid2"]],
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
    "result": "0100000001..."
}
```

### Response Parameters

| Field  | Type   | Description             |
| ------ | ------ | ----------------------- |
| result | string | Hex-encoded proof data. |

### Use Case

The `gettxoutproof` method is essential for:

* SPV verification
* Light wallet proofs
* Transaction inclusion proof
* Cross-chain verification

### Error Handling

| Status Code | Error Message | Cause                            |
| ----------- | ------------- | -------------------------------- |
| 403         | Forbidden     | Missing or invalid ACCESS-TOKEN. |

### Integration

The `gettxoutproof` method helps developers build robust applications that interact with the Litecoin blockchain.
