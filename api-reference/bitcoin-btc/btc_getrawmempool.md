---
description: >-
  Example code for the getrawmempool JSON-RPC method. Complete guide on how to
  use getrawmempool JSON-RPC in GetBlock Web3 documentation.
---

# getrawmempool - Bitcoin

This method returns all transaction ids in memory pool as a JSON array of strings.

### Parameters

| Parameter         | Type    | Required | Description                                                                           |
| ----------------- | ------- | -------- | ------------------------------------------------------------------------------------- |
| verbose           | boolean | No       | True for a JSON object, false for array of txids (default: false).                    |
| mempool\_sequence | boolean | No       | If verbose=false, returns a JSON object with txid array and mempool\_sequence number. |

### Request

{% tabs %}
{% tab title="Untitled" %}
```bash
curl --location --request POST 'https://go.getblock.io/<ACCESS-TOKEN>/' \
--header 'Content-Type: application/json' \
--data-raw '{
    "jsonrpc": "2.0",
    "method": "getrawmempool",
    "params": [false],
    "id": "getblock.io"
}'
```
{% endtab %}

{% tab title="Axios" %}
```javascript
import axios from 'axios';

const data = JSON.stringify({
    "jsonrpc": "2.0",
    "method": "getrawmempool",
    "params": [false],
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
    "method": "getrawmempool",
    "params": [False],
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
            "method": "getrawmempool",
            "params": [false],
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

### Response (Non-Verbose)

```json
{
    "jsonrpc": "2.0",
    "id": "getblock.io",
    "result": [
        "52273e0ce6cf3452932cfbc1c517c0ce1af1d255fda67a6e3bd63ba1d908c8c2",
        "abc123def456789012345678901234567890123456789012345678901234abcd",
        "def456abc789012345678901234567890123456789012345678901234567efgh"
    ]
}
```

### Response (Verbose)

```json
{
    "jsonrpc": "2.0",
    "id": "getblock.io",
    "result": {
        "52273e0ce6cf3452932cfbc1c517c0ce1af1d255fda67a6e3bd63ba1d908c8c2": {
            "vsize": 141,
            "weight": 561,
            "fee": 0.00001410,
            "modifiedfee": 0.00001410,
            "time": 1700000000,
            "height": 820000,
            "descendantcount": 1,
            "descendantsize": 141,
            "descendantfees": 1410,
            "ancestorcount": 1,
            "ancestorsize": 141,
            "ancestorfees": 1410,
            "wtxid": "abc123...",
            "fees": {
                "base": 0.00001410,
                "modified": 0.00001410,
                "ancestor": 0.00001410,
                "descendant": 0.00001410
            },
            "depends": [],
            "spentby": [],
            "bip125-replaceable": true
        }
    }
}
```

### Response Parameters

| Field                | Type    | Description                                              |
| -------------------- | ------- | -------------------------------------------------------- |
| result (non-verbose) | array   | Array of transaction ids.                                |
| result (verbose)     | object  | Object with txid keys and transaction details as values. |
| vsize                | number  | Virtual transaction size.                                |
| weight               | number  | Transaction weight.                                      |
| fee                  | number  | Transaction fee in BTC.                                  |
| modifiedfee          | number  | Fee with priority fee delta.                             |
| time                 | number  | Local time transaction entered pool.                     |
| height               | number  | Block height when transaction entered pool.              |
| descendantcount      | number  | Number of in-mempool descendants.                        |
| descendantsize       | number  | Virtual size of in-mempool descendants.                  |
| ancestorcount        | number  | Number of in-mempool ancestors.                          |
| ancestorsize         | number  | Virtual size of in-mempool ancestors.                    |
| depends              | array   | Unconfirmed transactions used as inputs.                 |
| spentby              | array   | Unconfirmed transactions spending this one's outputs.    |
| bip125-replaceable   | boolean | Whether BIP125 replaceable.                              |

### Use Case

The `getrawmempool` method is essential for:

* Monitoring pending transactions
* Building mempool explorers
* Analyzing transaction backlog
* Creating fee market analysis tools
* Implementing mempool synchronization
* Supporting block template creation

### Error Handling

| Status Code | Error Message | Cause                            |
| ----------- | ------------- | -------------------------------- |
| 403         | Forbidden     | Missing or invalid ACCESS-TOKEN. |

### Integration With Web3

The `getrawmempool` method helps developers:

* Build mempool browsers and explorers
* Create transaction monitoring systems
* Implement mempool analytics
* Track unconfirmed transaction volume
* Support mining software integration
