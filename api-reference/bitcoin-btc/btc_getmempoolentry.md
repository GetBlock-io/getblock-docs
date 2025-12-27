---
description: >-
  Example code for the getmempoolentry JSON-RPC method. Complete guide on how to
  use getmempoolentry JSON-RPC in GetBlock Web3 documentation.
---

# getmempoolentry - Bitcoin

This method returns mempool data for a given transaction in the mempool.

### Parameters

| Parameter | Type   | Required | Description                              |
| --------- | ------ | -------- | ---------------------------------------- |
| txid      | string | Yes      | The transaction id (must be in mempool). |

### Request

{% tabs %}
{% tab title="cURL" %}
```bash
curl --location --request POST 'https://go.getblock.io/<ACCESS-TOKEN>/' \
--header 'Content-Type: application/json' \
--data-raw '{
    "jsonrpc": "2.0",
    "method": "getmempoolentry",
    "params": ["42c239dac42f146396888e0db814f7489f8b7c1aa773887f0249d03531e868c9"],
    "id": "getblock.io"
}'
```
{% endtab %}

{% tab title="Axios" %}
```javascript
import axios from 'axios';

const data = JSON.stringify({
    "jsonrpc": "2.0",
    "method": "getmempoolentry",
    "params": ["52273e0ce6cf3452932cfbc1c517c0ce1af1d255fda67a6e3bd63ba1d908c8c2"],
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

```javascript
const message = "hello world";
console.log(message);
```
{% endtab %}

{% tab title="Request" %}
```python
import requests
import json

url = "https://go.getblock.io/<ACCESS-TOKEN>/"

payload = json.dumps({
    "jsonrpc": "2.0",
    "method": "getmempoolentry",
    "params": ["52273e0ce6cf3452932cfbc1c517c0ce1af1d255fda67a6e3bd63ba1d908c8c2"],
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
            "method": "getmempoolentry",
            "params": ["52273e0ce6cf3452932cfbc1c517c0ce1af1d255fda67a6e3bd63ba1d908c8c2"],
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
    "jsonrpc": "2.0",
    "id": "getblock.io",
    "result": {
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
        "wtxid": "abc123def456789...",
        "fees": {
            "base": 0.00001410,
            "modified": 0.00001410,
            "ancestor": 0.00001410,
            "descendant": 0.00001410
        },
        "depends": [],
        "spentby": [],
        "bip125-replaceable": true,
        "unbroadcast": false
    }
}
```

### Response Parameters

| Field              | Type    | Description                                           |
| ------------------ | ------- | ----------------------------------------------------- |
| vsize              | number  | Virtual transaction size.                             |
| weight             | number  | Transaction weight (BIP 141).                         |
| fee                | number  | Transaction fee in BTC.                               |
| modifiedfee        | number  | Fee with fee deltas used for mining priority.         |
| time               | number  | Local time transaction entered pool (Unix timestamp). |
| height             | number  | Block height when transaction entered pool.           |
| descendantcount    | number  | Number of in-mempool descendant transactions.         |
| descendantsize     | number  | Virtual size of in-mempool descendants.               |
| descendantfees     | number  | Modified fees of in-mempool descendants (satoshis).   |
| ancestorcount      | number  | Number of in-mempool ancestor transactions.           |
| ancestorsize       | number  | Virtual size of in-mempool ancestors.                 |
| ancestorfees       | number  | Modified fees of in-mempool ancestors (satoshis).     |
| wtxid              | string  | Hash of serialized transaction with witness data.     |
| fees               | object  | Fee breakdown object.                                 |
| depends            | array   | Unconfirmed transactions used as inputs.              |
| spentby            | array   | Unconfirmed transactions spending this one's outputs. |
| bip125-replaceable | boolean | Whether transaction is replaceable (BIP 125).         |
| unbroadcast        | boolean | Whether transaction is waiting to be broadcast.       |

### Use Case

The `getmempoolentry` method is essential for:

* Checking if a transaction is in the mempool
* Getting detailed fee information for unconfirmed transactions
* Monitoring transaction confirmation status
* Analyzing transaction priority
* Building transaction tracking systems
* Implementing fee estimation tools

### Error Handling

| Status Code | Error Message              | Cause                                     |
| ----------- | -------------------------- | ----------------------------------------- |
| 403         | Forbidden                  | Missing or invalid ACCESS-TOKEN.          |
| -5          | Transaction not in mempool | The specified txid is not in the mempool. |

### Integration With Web3

The `getmempoolentry` method helps developers:

* Build transaction status trackers
* Create confirmation time estimators
* Implement mempool monitoring dashboards
* Analyze transaction fee effectiveness
* Support RBF decision-making
