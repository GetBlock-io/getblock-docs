# getblockstats bitcoin

This method computes per-block statistics for a given block, identified by hash or height. Specific statistics can be selected with the stats parameter.

## Parameters

| Parameter        | Type             | Required | Description                                |
| ---------------- | ---------------- | -------- | ------------------------------------------ |
| hash\_or\_height | string or number | Yes      | The block hash or height                   |
| stats            | array            | No       | Values to return; returns all when omitted |

## Request

{% tabs %}
{% tab title="cURL" %}
{% code overflow="wrap" %}
```bash
curl --location --request POST 'https://shared.eu-central-1.getblock.io/<ACCESS-TOKEN>/' \
--header 'Content-Type: application/json' \
--data-raw '{
    "jsonrpc": "2.0",
    "method": "getblockstats",
    "params": [830000],
    "id": "getblock.io"
}'
```
{% endcode %}
{% endtab %}

{% tab title="bitcoinjs-lib" %}
{% code title="example.js" %}
```javascript
const axios = require('axios');

const response = await axios.post('https://shared.eu-central-1.getblock.io/<ACCESS-TOKEN>/', {
    jsonrpc: '2.0',
    method: 'getblockstats',
    params: [830000],
    id: 'getblock.io'
}, {
    headers: { 'Content-Type': 'application/json' }
});

console.log(response.data.result);
```
{% endcode %}
{% endtab %}

{% tab title="Request" %}
{% code title="example.py" %}
```python
import requests

response = requests.post(
    'https://shared.eu-central-1.getblock.io/<ACCESS-TOKEN>/',
    headers={'Content-Type': 'application/json'},
    json={
        'jsonrpc': '2.0',
        'method': 'getblockstats',
        'params': [830000],
        'id': 'getblock.io'
    }
)

print(response.json())
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
        .post("https://shared.eu-central-1.getblock.io/<ACCESS-TOKEN>/")
        .header("Content-Type", "application/json")
        .json(&json!({
            "jsonrpc": "2.0",
            "method": "getblockstats",
            "params": [830000],
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
    "jsonrpc": "2.0",
    "id": "getblock.io",
    "result": {
        "height": 830000,
        "time": 1706886000,
        "txs": 3201,
        "total_size": 1483920,
        "total_weight": 3943549,
        "totalfee": 12500000,
        "subsidy": 625000000,
        "avgfeerate": 24,
        "minfeerate": 1,
        "maxfeerate": 512,
        "avgtxsize": 462,
        "ins": 8123,
        "outs": 9542,
        "utxo_increase": 1419
    }
}
```

## Response Parameters

| Parameter | Type   | Description                             |
| --------- | ------ | --------------------------------------- |
| jsonrpc   | string | JSON-RPC protocol version ("2.0")       |
| id        | string | Request identifier matching the request |
| result    | object | Per-block statistics object             |

### Result Object

| Field          | Type   | Description                                     |
| -------------- | ------ | ----------------------------------------------- |
| txs            | number | Number of transactions in the block             |
| totalfee       | number | Total fees in the block, in satoshis            |
| subsidy        | number | Block subsidy (newly minted coins), in satoshis |
| avgfeerate     | number | Average fee rate in satoshis per virtual byte   |
| utxo\_increase | number | Net change in the number of UTXOs               |

## Use Cases

* **Fee Analytics**: Read average and range of fee rates per block
* **Block Economics**: Read subsidy and total fees
* **Capacity Analysis**: Read size, weight, and transaction counts
* **Dashboards**: Chart per-block statistics

## Error Handling

| Error Code | Message           | Description                                      |
| ---------- | ----------------- | ------------------------------------------------ |
| 403        | Forbidden         | Missing or invalid ACCESS-TOKEN                  |
| -5         | Not found         | The requested block or transaction was not found |
| -8         | Invalid parameter | A parameter is out of range or malformed         |
| -32602     | Invalid params    | A parameter is missing or has the wrong type     |
