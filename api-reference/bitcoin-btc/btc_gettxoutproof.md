---
description: >-
  Example code for the gettxoutproof JSON-RPC method. Complete guide on how to
  use gettxoutproof JSON-RPC in GetBlock Web3 documentation.
---

# gettxoutproof - Bitcoin

This method returns a hex-encoded proof that one or more transactions were included in a block.

### Parameters

| Parameter | Type   | Required | Description                                     |
| --------- | ------ | -------- | ----------------------------------------------- |
| txids     | array  | Yes      | Array of transaction ids to filter.             |
| blockhash | string | No       | The block hash to look for the transactions in. |

### Request

{% tabs %}
{% tab title="cURL" %}
```bash
curl --location --request POST 'https://go.getblock.io/<ACCESS-TOKEN>/' \
--header 'Content-Type: application/json' \
--data-raw '{
    "jsonrpc": "2.0",
    "method": "gettxoutproof",
    "params": [["ee652f0b40209bd02468de0c6336854f5efdd79fb865560aef2c46f4fa0b4a07"]],
    "id": "getblock.io"
}'
```
{% endtab %}

{% tab title="Axios" %}
```javascript
import axios from 'axios';

const data = JSON.stringify({
    "jsonrpc": "2.0",
    "method": "gettxoutproof",
    "params": [["ee652f0b40209bd02468de0c6336854f5efdd79fb865560aef2c46f4fa0b4a07"]],
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
    "method": "gettxoutproof",
    "params": [["ee652f0b40209bd02468de0c6336854f5efdd79fb865560aef2c46f4fa0b4a07"]],
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
{% code overflow="wrap" %}
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
            "params": [["ee652f0b40209bd02468de0c6336854f5efdd79fb865560aef2c46f4fa0b4a07"]],
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
    "jsonrpc": "2.0",
    "id": "getblock.io",
    "result": "00000020a9b2c3d4e5f6789abcdef0123456789abcdef01234567890abcdef1234567890abcdef01234567..."
}
```
{% endcode %}

### Response Parameters

| Field  | Type   | Description                                                                          |
| ------ | ------ | ------------------------------------------------------------------------------------ |
| result | string | Hex-encoded serialized Merkle proof connecting the transactions to the block header. |

### Use Case

The `gettxoutproof` method is essential for:

* SPV (Simplified Payment Verification) wallet implementations
* Lightweight client transaction verification
* Proof generation for cross-chain bridges
* Building trustless payment verification
* Creating compact transaction proofs
* Supporting light node protocols

### Error Handling

| Status Code | Error Message                | Cause                                    |
| ----------- | ---------------------------- | ---------------------------------------- |
| 403         | Forbidden                    | Missing or invalid ACCESS-TOKEN.         |
| -5          | Transaction not yet in block | Transaction is unconfirmed or not found. |
| -8          | Block not found              | The specified blockhash does not exist.  |

### Integration With Web3

The `gettxoutproof` method helps developers:

* Build SPV wallet verification
* Create lightweight payment proof systems
* Implement cross-chain transaction verification
* Support trustless bridge protocols
* Generate Merkle proofs for auditing
