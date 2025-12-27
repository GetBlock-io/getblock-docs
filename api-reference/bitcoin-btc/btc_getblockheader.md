---
description: >-
  Example code for the getblockheader JSON-RPC method. Complete guide on how to
  use getblockheader JSON-RPC in GetBlock Web3 documentation.
---

# getblockheader - Bitcoin

This method returns information about a block header given its hash.

### Parameters

| Parameter | Type    | Required | Description                                                         |
| --------- | ------- | -------- | ------------------------------------------------------------------- |
| blockhash | string  | Yes      | The block hash.                                                     |
| verbose   | boolean | No       | true for a JSON object, false for hex-encoded data (default: true). |

### Request

{% tabs %}
{% tab title="cURL" %}
```bash
curl --location --request POST 'https://go.getblock.io/<ACCESS-TOKEN>/' \
--header 'Content-Type: application/json' \
--data-raw '{
    "jsonrpc": "2.0",
    "method": "getblockheader",
    "params": ["000000000000000000046b9302e08c16ea186950f42a5498320ddd1bd7ab3428", true],
    "id": "getblock.io"
}'
```
{% endtab %}

{% tab title="Axios" %}
```javascript
import axios from 'axios';

const data = JSON.stringify({
    "jsonrpc": "2.0",
    "method": "getblockheader",
    "params": ["000000000000000000046b9302e08c16ea186950f42a5498320ddd1bd7ab3428", true],
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
    "method": "getblockheader",
    "params": ["000000000000000000046b9302e08c16ea186950f42a5498320ddd1bd7ab3428", True],
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
            "method": "getblockheader",
            "params": ["000000000000000000046b9302e08c16ea186950f42a5498320ddd1bd7ab3428", true],
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
    "result": {
        "hash": "0000000000000000000454a3a654c88ab5ad9824ca8506c1f7f65cc0ea193503",
        "confirmations": 143923,
        "height": 784665,
        "version": 683581440,
        "versionHex": "28bea000",
        "merkleroot": "d8f4f9edd1c0a37a705289b7202a3d3b67ef46a20f88c27c88fe757aa87f17c5",
        "time": 1681051849,
        "mediantime": 1681049210,
        "nonce": 2450381578,
        "bits": "1705e0b2",
        "difficulty": 47887764338536.25,
        "chainwork": "000000000000000000000000000000000000000045155fb82c72a339d3522688",
        "nTx": 169,
        "previousblockhash": "00000000000000000003a77bfe6ccd653f7db680c4a03fa4b78ac95fa9dd5538",
        "nextblockhash": "000000000000000000028b6ee09959abd8a47ce1487f3854fda0a7ed0b4e85ae"
    },
    "error": null
}
```

### Response Parameters

| Field             | Type   | Description                                          |
| ----------------- | ------ | ---------------------------------------------------- |
| hash              | string | The block hash.                                      |
| confirmations     | number | Number of confirmations, or -1 if not in main chain. |
| height            | number | Block height or index.                               |
| version           | number | Block version.                                       |
| versionHex        | string | Block version formatted in hexadecimal.              |
| merkleroot        | string | The merkle root.                                     |
| time              | number | Block time as Unix epoch time.                       |
| mediantime        | number | Median time of the previous 11 blocks.               |
| nonce             | number | The nonce.                                           |
| bits              | string | The bits (compact target).                           |
| difficulty        | number | The difficulty.                                      |
| chainwork         | string | Expected number of hashes to produce current chain.  |
| nTx               | number | Number of transactions in the block.                 |
| previousblockhash | string | Hash of the previous block.                          |
| nextblockhash     | string | Hash of the next block (if available).               |

### Use Case

The `getblockheader` method is essential for:

* Light client block validation
* Efficient block metadata retrieval
* SPV proof verification
* Mining pool header fetching
* Block time analysis
* Chain difficulty tracking

### Error Handling

| Status Code | Error Message             | Cause                                                        |
| ----------- | ------------------------- | ------------------------------------------------------------ |
| 403         | Forbidden                 | Missing or invalid ACCESS-TOKEN.                             |
| -8          | 500 internal server error | The specified block hash does not exist or hash not up to 64 |

### Integration Notes

The `getblockheader` method helps developers:

* Implement lightweight block verification
* Build efficient chain scanners
* Support SPV wallets
* Create mining pool interfaces
* Build header chain synchronization
