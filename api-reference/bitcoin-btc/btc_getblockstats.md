---
description: >-
  Example code for the getblockstats JSON-RPC method. Complete guide on how to
  use getblockstats JSON-RPC in GetBlock Web3 documentation.
---

# getblockstats - Bitcoin

This method computes per-block statistics for a given block or block range.

### Parameters

| Parameter        | Type          | Required | Description                                                          |
| ---------------- | ------------- | -------- | -------------------------------------------------------------------- |
| hash\_or\_height | string/number | Yes      | The block hash or height.                                            |
| stats            | array         | No       | Array of stat names to retrieve. Returns all stats if not specified. |

### Request

{% tabs %}
{% tab title="cURL" %}
```bash
curl --location --request POST 'https://go.getblock.io/<ACCESS-TOKEN>/' \
--header 'Content-Type: application/json' \
--data-raw '{
    "jsonrpc": "2.0",
    "method": "getblockstats",
    "params": [820000, ["txs", "avgfee", "avgfeerate", "height", "time"]],
    "id": "getblock.io"
}'
```
{% endtab %}

{% tab title="Axios" %}
```javascript
import axios from 'axios';

const data = JSON.stringify({
    "jsonrpc": "2.0",
    "method": "getblockstats",
    "params": [820000, ["txs", "avgfee", "avgfeerate", "height", "time"]],
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
    "method": "getblockstats",
    "params": [820000, ["txs", "avgfee", "avgfeerate", "height", "time"]],
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
            "method": "getblockstats",
            "params": [820000, ["txs", "avgfee", "avgfeerate", "height", "time"]],
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
        "avgfee": 25000,
        "avgfeerate": 45,
        "height": 820000,
        "time": 1700000000,
        "txs": 3500
    }
}
```

### Response Parameters (Full Stats)

| Field           | Type   | Description                                |
| --------------- | ------ | ------------------------------------------ |
| avgfee          | number | Average fee per transaction (in satoshis). |
| avgfeerate      | number | Average feerate (in sat/vB).               |
| avgtxsize       | number | Average transaction size.                  |
| blockhash       | string | The block hash.                            |
| height          | number | Block height.                              |
| ins             | number | Total number of inputs.                    |
| maxfee          | number | Maximum fee in the block.                  |
| maxfeerate      | number | Maximum feerate in the block.              |
| maxtxsize       | number | Maximum transaction size.                  |
| medianfee       | number | Median transaction fee.                    |
| mediantime      | number | Block median time.                         |
| mediantxsize    | number | Median transaction size.                   |
| minfee          | number | Minimum fee in the block.                  |
| minfeerate      | number | Minimum feerate in the block.              |
| mintxsize       | number | Minimum transaction size.                  |
| outs            | number | Total number of outputs.                   |
| subsidy         | number | Block subsidy (in satoshis).               |
| swtotal\_size   | number | Total size of SegWit transactions.         |
| swtotal\_weight | number | Total weight of SegWit transactions.       |
| swtxs           | number | Number of SegWit transactions.             |
| time            | number | Block timestamp.                           |
| total\_out      | number | Total output value.                        |
| total\_size     | number | Total size of all transactions.            |
| total\_weight   | number | Total weight of all transactions.          |
| totalfee        | number | Total fees in the block.                   |
| txs             | number | Number of transactions.                    |
| utxo\_increase  | number | Change in UTXO set size.                   |
| utxo\_size\_inc | number | Change in UTXO set serialized size.        |

### Use Case

The `getblockstats` method is essential for:

* Block analysis and visualization
* Fee market research
* Transaction volume tracking
* Mining profitability analysis
* SegWit adoption monitoring
* Building blockchain analytics dashboards

### Error Handling

| Status Code | Error Message                         | Cause                                       |
| ----------- | ------------------------------------- | ------------------------------------------- |
| 403         | Forbidden                             | Missing or invalid ACCESS-TOKEN.            |
| -3          | Block not inputed                     | The block not inputed.                      |
| -8          | Invalid stat name or Block not found  | One of the requested stat names is invalid. |

### Integration Notes

The `getblockstats` method helps developers:

* Build block analytics platforms
* Create fee estimation models
* Monitor SegWit adoption
* Analyze transaction patterns
* Track blockchain economics
