---
description: >-
  Example code for the getblockstats JSON-RPC method. Complete guide on how to
  use getblockstats JSON-RPC in GetBlock Web3 documentation.
---

# getblockstats - Bitcoin Cash

This method computes per-block statistics for a block identified by hash or height. A subset of statistics can be requested to reduce computation.

## Parameters

| Parameter        | Type              | Required | Description                              |
| ---------------- | ----------------- | -------- | ---------------------------------------- |
| hash\_or\_height | string or numeric | Yes      | Block hash or height of the target block |
| stats            | array             | No       | Values to return. Default is all values  |

## Request

{% tabs %}
{% tab title="cURL" %}
{% code overflow="wrap" %}
```bash
curl --location --request POST 'https://go.getblock.io/<ACCESS-TOKEN>/' \
--header 'Content-Type: application/json' \
--data-raw '{
    "jsonrpc": "2.0",
    "method": "getblockstats",
    "params": [684634, null],
    "id": "getblock.io"
}'
```
{% endcode %}
{% endtab %}

{% tab title="bitcoinjs-lib" %}
{% code title="example.js" %}
```javascript
const axios = require('axios');
const { data } = await axios.post('https://go.getblock.io/<ACCESS-TOKEN>/', {
  jsonrpc: '2.0', method: 'getblockstats',
  params: [684634, null], id: 'getblock.io'
});
console.log(data.result.avgfeerate, data.result.txs);
```
{% endcode %}
{% endtab %}

{% tab title="Request" %}
{% code title="example.py" %}
```python
import requests

response = requests.post(
    'https://go.getblock.io/<ACCESS-TOKEN>/',
    headers={'Content-Type': 'application/json'},
    json={
        'jsonrpc': '2.0',
        'method': 'getblockstats',
        'params': [684634, null],
        'id': 'getblock.io'
    }
)

print(response.json()['result'])
```
{% endcode %}
{% endtab %}

{% tab title="Rust" %}
{% code title="example.rs" %}
```rust
use reqwest::Client;
use serde_json::{json, Value};

#[tokio::main]
async fn main() -> Result<(), Box<dyn std::error::Error>> {
    let client = Client::new();

    let response = client
        .post("https://go.getblock.io/<ACCESS-TOKEN>/")
        .header("Content-Type", "application/json")
        .json(&json!({
            "jsonrpc": "2.0",
            "method": "getblockstats",
            "params": [684634, null],
            "id": "getblock.io"
        }))
        .send()
        .await?
        .json::<Value>()
        .await?;

    println!("Result: {}", response["result"]);
    Ok(())
}
```
{% endcode %}
{% endtab %}
{% endtabs %}

## Response

```json
{
    "error": null,
    "id": "getblock.io",
    "result": {
        "avgfee": 4520,
        "avgfeerate": 12,
        "avgtxsize": 445,
        "blockhash": "0000000000000000023a561e1ea370153aac5d1504726d1a039032831c05fcfc",
        "height": 684634,
        "ins": 4821,
        "maxfee": 182000,
        "maxfeerate": 220,
        "medianfee": 2260,
        "mediantime": 1617176373,
        "minfee": 500,
        "outs": 5932,
        "subsidy": 625000000,
        "time": 1617180599,
        "total_out": 284739201847,
        "total_size": 1252030,
        "totalfee": 12730055,
        "txs": 2815,
        "utxo_increase": 1111
    }
}
```

## Response Parameters

| Parameter | Type         | Description                                      |
| --------- | ------------ | ------------------------------------------------ |
| error     | null\|object | Error object when the call fails, otherwise null |
| id        | string       | Request identifier matching the request          |
| result    | object       | Object of computed statistics for the block      |

### Result Object

| Field      | Type    | Description                                   |
| ---------- | ------- | --------------------------------------------- |
| avgfee     | numeric | Average fee in the block, in satoshis         |
| avgfeerate | numeric | Average fee rate in satoshis per virtual byte |
| height     | numeric | Block height                                  |
| maxfeerate | numeric | Maximum fee rate in the block                 |
| subsidy    | numeric | Block subsidy in satoshis                     |
| totalfee   | numeric | Total fees in the block, in satoshis          |
| txs        | numeric | Number of transactions in the block           |

## Use Cases

* **Fee Estimation**: Derive recent fee-rate distributions from recent blocks
* **Block Analytics**: Chart transaction counts and sizes over a height range
* **Subsidy Tracking**: Read the block subsidy to model issuance across halvings
* **Capacity Studies**: Measure UTXO set growth per block over time

## Error Handling

| Error Code | Message                   | Description                                             |
| ---------- | ------------------------- | ------------------------------------------------------- |
| -8         | Block height out of range | The requested height is negative or above the chain tip |
| -1         | Invalid parameter type    | height is not an integer                                |
| -32603     | Internal error            | Node failed to read the block index                     |
