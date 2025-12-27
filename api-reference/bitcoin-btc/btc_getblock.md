---
description: >-
  Example code for the getblock JSON-RPC method. Complete guide on how to use
  getblock JSON-RPC in GetBlock Web3 documentation.
---

# getblock - Bitcoin

This method returns information about a block given its hash. The amount of detail returned depends on the verbosity level.

### Parameters

| Parameter | Type   | Required | Description                                                                                      |
| --------- | ------ | -------- | ------------------------------------------------------------------------------------------------ |
| blockhash | string | Yes      | The block hash.                                                                                  |
| verbosity | number | No       | 0 for hex-encoded data, 1 for JSON object, 2 for JSON object with transaction data (default: 1). |

### Request

{% tabs %}
{% tab title="cURL" %}
```bash
curl --location --request POST 'https://go.getblock.io/<ACCESS-TOKEN>/' \
--header 'Content-Type: application/json' \
--data-raw '{
    "jsonrpc": "2.0",
    "method": "getblock",
    "params": ["000000000000000000046b9302e08c16ea186950f42a5498320ddd1bd7ab3428", 1],
    "id": "getblock.io"
}'
```


{% endtab %}

{% tab title="Axios" %}
```javascript
import axios from 'axios';

const data = JSON.stringify({
    "jsonrpc": "2.0",
    "method": "getblock",
    "params": ["000000000000000000046b9302e08c16ea186950f42a5498320ddd1bd7ab3428", 1],
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
    "method": "getblock",
    "params": ["000000000000000000046b9302e08c16ea186950f42a5498320ddd1bd7ab3428", 1],
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
            "method": "getblock",
            "params": ["000000000000000000046b9302e08c16ea186950f42a5498320ddd1bd7ab3428", 1],
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
    "result": {
        "hash": "000000000000000000046b9302e08c16ea186950f42a5498320ddd1bd7ab3428",
        "confirmations": 15234,
        "height": 820000,
        "version": 536870912,
        "versionHex": "20000000",
        "merkleroot": "2d66d4b9d56b5f3a3e5ef65323f19b8f68c48e9a4b6d1a7c8e2f3b4c5d6e7f8a",
        "time": 1700000000,
        "mediantime": 1699999000,
        "nonce": 1234567890,
        "bits": "170d21b9",
        "difficulty": 62460000000000,
        "chainwork": "000000000000000000000000000000000000000050a1b2c3d4e5f6789abcdef0",
        "nTx": 3500,
        "previousblockhash": "000000000000000000047a9e02e08c16ea186950f42a5498320ddd1bd7ab3427",
        "nextblockhash": "000000000000000000045b8302e08c16ea186950f42a5498320ddd1bd7ab3429",
        "strippedsize": 998000,
        "size": 1500000,
        "weight": 3993000,
        "tx": [
            "txid1...",
            "txid2...",
            "..."
        ]
    }
}
```

### Response Parameters

| Field             | Type   | Description                                                                  |
| ----------------- | ------ | ---------------------------------------------------------------------------- |
| hash              | string | The block hash.                                                              |
| confirmations     | number | Number of confirmations, or -1 if not in main chain.                         |
| height            | number | Block height or index.                                                       |
| version           | number | Block version.                                                               |
| versionHex        | string | Block version formatted in hexadecimal.                                      |
| merkleroot        | string | The merkle root of transactions.                                             |
| time              | number | Block time as Unix epoch time.                                               |
| mediantime        | number | Median time of previous 11 blocks.                                           |
| nonce             | number | The nonce value.                                                             |
| bits              | string | The bits field (compact target).                                             |
| difficulty        | number | The mining difficulty.                                                       |
| chainwork         | string | Expected number of hashes to produce current chain.                          |
| nTx               | number | Number of transactions in the block.                                         |
| previousblockhash | string | Hash of the previous block.                                                  |
| nextblockhash     | string | Hash of the next block (if available).                                       |
| strippedsize      | number | Size of block without witness data.                                          |
| size              | number | Block size in bytes.                                                         |
| weight            | number | Block weight (BIP 141).                                                      |
| tx                | array  | Array of transaction ids (verbosity=1) or transaction objects (verbosity=2). |

### Use Case

The `getblock` method is essential for:

* Building blockchain explorers
* Analyzing block contents and statistics
* Tracking transaction confirmations
* Monitoring mining activity
* Building chain analysis tools
* Creating block visualization dashboards

### Error Handling

| Status Code | Error Message   | Cause                                    |
| ----------- | --------------- | ---------------------------------------- |
| 403         | Forbidden       | Missing or invalid ACCESS-TOKEN.         |
| -5          | Block not found | The specified block hash does not exist. |

### Integration Notes

The `getblock` method helps developers:

* Build full-featured blockchain explorers
* Implement block notification services
* Create mining pool monitoring tools
* Support transaction confirmation tracking
* Build chain analysis applications
