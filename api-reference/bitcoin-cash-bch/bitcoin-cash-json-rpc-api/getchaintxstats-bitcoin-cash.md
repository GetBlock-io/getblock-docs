---
description: >-
  Example code for the getchaintxstats JSON-RPC method. Complete guide on how to
  use getchaintxstats JSON-RPC in GetBlock Web3 documentation.
---

# getchaintxstats - Bitcoin Cash

This method computes statistics about the total number and rate of transactions in the chain over a window of blocks ending at a given block.

## Parameters

| Parameter | Type    | Required | Description                                                      |
| --------- | ------- | -------- | ---------------------------------------------------------------- |
| nblocks   | numeric | No       | Size of the window in blocks. Default is one month of blocks     |
| blockhash | string  | No       | Hash of the block that ends the window. Default is the chain tip |

## Request

{% tabs %}
{% tab title="cURL" %}
{% code overflow="wrap" %}
```bash
curl --location --request POST 'https://go.getblock.io/<ACCESS-TOKEN>/' \
--header 'Content-Type: application/json' \
--data-raw '{
    "jsonrpc": "2.0",
    "method": "getchaintxstats",
    "params": [30, "0000000000000000023a561e1ea370153aac5d1504726d1a039032831c05fcfc"],
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
  jsonrpc: '2.0', method: 'getchaintxstats',
  params: [30, '0000000000000000023a561e1ea370153aac5d1504726d1a039032831c05fcfc'], id: 'getblock.io'
});
console.log(data.result.txrate);
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
        'method': 'getchaintxstats',
        'params': [30, "0000000000000000023a561e1ea370153aac5d1504726d1a039032831c05fcfc"],
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
            "method": "getchaintxstats",
            "params": [30, "0000000000000000023a561e1ea370153aac5d1504726d1a039032831c05fcfc"],
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
        "time": 1617180599,
        "txcount": 331847291,
        "window_final_block_hash": "0000000000000000023a561e1ea370153aac5d1504726d1a039032831c05fcfc",
        "window_final_block_height": 684634,
        "window_block_count": 30,
        "window_tx_count": 84213,
        "window_interval": 18240,
        "txrate": 4.618037
    }
}
```

## Response Parameters

| Parameter | Type         | Description                                                |
| --------- | ------------ | ---------------------------------------------------------- |
| error     | null\|object | Error object when the call fails, otherwise null           |
| id        | string       | Request identifier matching the request                    |
| result    | object       | Object of transaction statistics over the requested window |

### Result Object

| Field             | Type    | Description                                           |
| ----------------- | ------- | ----------------------------------------------------- |
| txcount           | numeric | Total transactions in the chain up to the final block |
| window\_tx\_count | numeric | Transactions in the window                            |
| window\_interval  | numeric | Elapsed time of the window in seconds                 |
| txrate            | numeric | Average transactions per second over the window       |

## Use Cases

* **Throughput Metrics**: Report average transactions per second on a status page
* **Trend Analysis**: Compare transaction rates across different windows
* **Capacity Planning**: Assess sustained load when sizing infrastructure
* **Network Reports**: Publish verified on-chain activity figures

## Error Handling

| Error Code | Message         | Description                                   |
| ---------- | --------------- | --------------------------------------------- |
| -32700     | Parse error     | Request body is not valid JSON                |
| -32600     | Invalid request | The JSON sent is not a valid request object   |
| -32603     | Internal error  | Node failed to read the requested chain state |
