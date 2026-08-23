# getchaintxstats bitcoin

This method computes statistics about the total number and rate of transactions in the chain over a window of blocks ending at a given block.

## Parameters

| Parameter | Type   | Required | Description                                        |
| --------- | ------ | -------- | -------------------------------------------------- |
| nblocks   | number | No       | Window size in blocks (default: one month)         |
| blockhash | string | No       | The window's final block hash (default: chain tip) |

## Request

{% tabs %}
{% tab title="cURL" %}
{% code overflow="wrap" %}
```bash
curl --location --request POST 'https://shared.eu-central-1.getblock.io/<ACCESS-TOKEN>/' \
--header 'Content-Type: application/json' \
--data-raw '{
    "jsonrpc": "2.0",
    "method": "getchaintxstats",
    "params": [],
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
    method: 'getchaintxstats',
    params: [],
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
        'method': 'getchaintxstats',
        'params': [],
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
            "method": "getchaintxstats",
            "params": [],
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
        "time": 1706886000,
        "txcount": 1012345678,
        "window_final_block_hash": "000000000000000000046b9302e08c16ea186950f42a5498320ddd1bd7ab3428",
        "window_final_block_height": 830000,
        "window_block_count": 4320,
        "window_tx_count": 13456789,
        "window_interval": 2592000,
        "txrate": 5.19
    }
}
```

## Response Parameters

| Parameter | Type   | Description                             |
| --------- | ------ | --------------------------------------- |
| jsonrpc   | string | JSON-RPC protocol version ("2.0")       |
| id        | string | Request identifier matching the request |
| result    | object | Chain transaction statistics object     |

### Result Object

| Field             | Type   | Description                                               |
| ----------------- | ------ | --------------------------------------------------------- |
| txcount           | number | Total number of transactions in the chain up to the block |
| window\_tx\_count | number | Transactions in the window                                |
| window\_interval  | number | Window length in seconds                                  |
| txrate            | number | Average transactions per second in the window             |

## Use Cases

* **Throughput Metrics**: Read average transactions per second
* **Growth Tracking**: Track total transaction count over time
* **Capacity Planning**: Analyze historical demand
* **Dashboards**: Chart network activity

## Error Handling

| Error Code | Message           | Description                                  |
| ---------- | ----------------- | -------------------------------------------- |
| 403        | Forbidden         | Missing or invalid ACCESS-TOKEN              |
| -8         | Invalid parameter | A parameter is out of range or malformed     |
| -32601     | Method not found  | The method is not available on this endpoint |
| -32602     | Invalid params    | A parameter is missing or has the wrong type |
