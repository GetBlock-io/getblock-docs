---
description: >-
  Example code for the getblock JSON-RPC method. Complete guide on how to use
  getblock JSON-RPC in GetBlock Web3 documentation.
---

# getblock - Litecoin

This method returns an object with information about the block for the given hash.

### Parameters

| Parameter | Type         | Description                                                                                                                                                                                                                      |
| --------- | ------------ | -------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| blockhash | string (hex) | The hash of the block to get, encoded as hex in RPC byte order.                                                                                                                                                                  |
| verbosity | number (int) | Optional. Set to one of the following verbosity levels: 0 - Get the block in serialized block format; 1 - Get the decoded block as a JSON object (default); 2 - Get the decoded block as a JSON object with transaction details. |

### Request

{% tabs %}
{% tab title="cURL" %}
```bash
curl --location --request POST 'https://go.getblock.io/<ACCESS-TOKEN>/' \
--header 'Content-Type: application/json' \
--data-raw '{
    "jsonrpc": "2.0",
    "method": "getblock",
    "params": ["a1b2c3d4e5f6...blockhash...", 1],
    "id": "getblock.io"
}'
```
{% endtab %}

{% tab title="Axios (JavaScript)" %}
{% code title="axios.js" %}
```javascript
import axios from 'axios';

const data = JSON.stringify({
    "jsonrpc": "2.0",
    "method": "getblock",
    "params": ["a1b2c3d4e5f6...blockhash...", 1],
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
    "method": "getblock",
    "params": ["a1b2c3d4e5f6...blockhash...", 1],
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
            "method": "getblock",
            "params": ["a1b2c3d4e5f6...blockhash...", 1],
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
        "hash": "a1b2c3d4e5f6...blockhash...",
        "confirmations": 100,
        "strippedsize": 228,
        "size": 264,
        "weight": 948,
        "height": 2750000,
        "version": 536870912,
        "versionHex": "20000000",
        "merkleroot": "4a5b6c7d8e9f...",
        "tx": ["txid1", "txid2"],
        "time": 1640000000,
        "mediantime": 1639999000,
        "nonce": 123456789,
        "bits": "1a0fffff",
        "difficulty": 12345678.90,
        "chainwork": "0000000000000000000000000000000000000000000000000000000000000001",
        "nTx": 2,
        "previousblockhash": "0000000000000000000000000000000000000000000000000000000000000000",
        "nextblockhash": "1111111111111111111111111111111111111111111111111111111111111111"
    }
}
```
{% endcode %}

### Response Parameters

| Field             | Type   | Description                                              |
| ----------------- | ------ | -------------------------------------------------------- |
| hash              | string | The block hash (same as provided).                       |
| confirmations     | number | The number of confirmations, or -1 if not in main chain. |
| size              | number | The block size in bytes.                                 |
| height            | number | The block height or index.                               |
| version           | number | The block version.                                       |
| merkleroot        | string | The merkle root.                                         |
| tx                | array  | The transaction ids.                                     |
| time              | number | The block time in seconds since epoch.                   |
| nonce             | number | The nonce.                                               |
| bits              | string | The bits.                                                |
| difficulty        | number | The difficulty.                                          |
| previousblockhash | string | The hash of the previous block.                          |
| nextblockhash     | string | The hash of the next block.                              |

### Use Case

The `getblock` method is essential for:

* Block explorers and chain analysis tools
* Transaction verification and auditing
* Historical data extraction
* Block validation
* Chain reorganization detection

### Error Handling

| Status Code | Error Message   | Cause                            |
| ----------- | --------------- | -------------------------------- |
| 403         | Forbidden       | Missing or invalid ACCESS-TOKEN. |
| -5          | Block not found | Invalid block hash provided.     |

### Integration Notes

The `getblock` method helps developers:

* Build block explorer functionality
* Verify transaction inclusion
* Analyze block composition
* Track chain progression
* Extract mining statistics
