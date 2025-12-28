---
description: >-
  Example code for the getmempoolancestors JSON-RPC method. Complete guide on
  how to use getmempoolancestors JSON-RPC in GetBlock Web3 documentation.
---

# getmempooldescendants - Bitcoin

This method returns all in-mempool ancestors of a transaction if it is in the mempool.

### Parameters

| Parameter | Type    | Required | Description                                                        |
| --------- | ------- | -------- | ------------------------------------------------------------------ |
| txid      | string  | Yes      | The transaction id (must be in mempool).                           |
| verbose   | boolean | No       | True for a JSON object, false for array of txids (default: false). |

### Request

{% tabs %}
{% tab title="cURL" %}
{% code overflow="wrap" %}
```bash
curl --location --request POST 'https://go.getblock.io/<ACCESS-TOKEN>/' \
--header 'Content-Type: application/json' \
--data-raw '{
    "jsonrpc": "2.0",
    "method": "getmempoolancestors",
    "params": ["52273e0ce6cf3452932cfbc1c517c0ce1af1d255fda67a6e3bd63ba1d908c8c2", true],
    "id": "getblock.io"
}'
```
{% endcode %}
{% endtab %}

{% tab title="Axios" %}
{% code overflow="wrap" %}
```javascript
import axios from 'axios';

const data = JSON.stringify({
    "jsonrpc": "2.0",
    "method": "getmempoolancestors",
    "params": ["52273e0ce6cf3452932cfbc1c517c0ce1af1d255fda67a6e3bd63ba1d908c8c2", true],
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

{% tab title="Request" %}
{% code overflow="wrap" %}
```python
import requests
import json

url = "https://go.getblock.io/<ACCESS-TOKEN>/"

payload = json.dumps({
    "jsonrpc": "2.0",
    "method": "getmempoolancestors",
    "params": ["52273e0ce6cf3452932cfbc1c517c0ce1af1d255fda67a6e3bd63ba1d908c8c2", True],
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
            "method": "getmempoolancestors",
            "params": ["52273e0ce6cf3452932cfbc1c517c0ce1af1d255fda67a6e3bd63ba1d908c8c2", true],
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
        "abc123def456...": {
            "vsize": 141,
            "weight": 561,
            "fee": 0.00001410,
            "modifiedfee": 0.00001410,
            "time": 1700000000,
            "height": 820000,
            "descendantcount": 2,
            "descendantsize": 282,
            "descendantfees": 2820,
            "ancestorcount": 1,
            "ancestorsize": 141,
            "ancestorfees": 1410,
            "wtxid": "def456abc123...",
            "fees": {
                "base": 0.00001410,
                "modified": 0.00001410,
                "ancestor": 0.00001410,
                "descendant": 0.00002820
            },
            "depends": [],
            "spentby": ["52273e0ce6cf3452932cfbc1c517c0ce1af1d255fda67a6e3bd63ba1d908c8c2"],
            "bip125-replaceable": true,
            "unbroadcast": false
        }
    }
}
```

### Response Parameters

| Field              | Type    | Description                                           |
| ------------------ | ------- | ----------------------------------------------------- |
| vsize              | number  | Virtual transaction size.                             |
| weight             | number  | Transaction weight.                                   |
| fee                | number  | Transaction fee in BTC.                               |
| modifiedfee        | number  | Fee with fee deltas used for mining priority.         |
| time               | number  | Local time when transaction entered pool.             |
| height             | number  | Block height when transaction entered pool.           |
| descendantcount    | number  | Number of in-mempool descendants.                     |
| descendantsize     | number  | Virtual size of in-mempool descendants.               |
| descendantfees     | number  | Fees of in-mempool descendants (in satoshis).         |
| ancestorcount      | number  | Number of in-mempool ancestors.                       |
| ancestorsize       | number  | Virtual size of in-mempool ancestors.                 |
| ancestorfees       | number  | Fees of in-mempool ancestors (in satoshis).           |
| wtxid              | string  | Witness transaction id.                               |
| fees               | object  | Fee information breakdown.                            |
| depends            | array   | Unconfirmed transactions used as inputs.              |
| spentby            | array   | Unconfirmed transactions spending this one's outputs. |
| bip125-replaceable | boolean | Whether transaction is replaceable (BIP 125).         |

### Use Case

The `getmempoolancestors` method is essential for:

* Analyzing transaction chains in mempool
* Calculating CPFP (Child Pays for Parent) effectiveness
* Understanding transaction dependencies
* Building mempool visualization tools
* Debugging stuck transactions
* Implementing RBF strategies

### Error Handling

| Status Code | Error Message              | Cause                                     |
| ----------- | -------------------------- | ----------------------------------------- |
| 403         | Forbidden                  | Missing or invalid ACCESS-TOKEN.          |
| -5          | Transaction not in mempool | The specified txid is not in the mempool. |

### Integration With Web3

The `getmempoolancestors` method helps developers:

* Build CPFP fee calculators
* Create mempool dependency visualizers
* Implement transaction acceleration tools
* Debug unconfirmed transaction chains
* Analyze fee bumping strategies
