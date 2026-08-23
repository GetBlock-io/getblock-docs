---
description: >-
  Example code for the getmempoolinfo JSON-RPC method. Complete guide on how to
  use getmempoolinfo JSON-RPC in GetBlock Web3 documentation.
---

# getmempoolinfo - Bitcoin

This method returns details about the active state of the transaction memory pool, including the number of transactions, total size, and minimum fee rate.

## Parameters

This method does not accept any parameters.

## Request

{% tabs %}
{% tab title="cURL" %}
{% code overflow="wrap" %}
```bash
curl --location --request POST 'https://shared.eu-central-1.getblock.io/<ACCESS-TOKEN>/' \
--header 'Content-Type: application/json' \
--data-raw '{
    "jsonrpc": "2.0",
    "method": "getmempoolinfo",
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
    method: 'getmempoolinfo',
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
        'method': 'getmempoolinfo',
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
            "method": "getmempoolinfo",
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
        "loaded": true,
        "size": 42150,
        "bytes": 28734512,
        "usage": 96345120,
        "total_fee": 1.23456789,
        "maxmempool": 300000000,
        "mempoolminfee": 1e-05,
        "minrelaytxfee": 1e-05,
        "unbroadcastcount": 3
    }
}
```

## Response Parameters

| Parameter | Type   | Description                             |
| --------- | ------ | --------------------------------------- |
| jsonrpc   | string | JSON-RPC protocol version ("2.0")       |
| id        | string | Request identifier matching the request |
| result    | object | Mempool state object                    |

### Result Object

| Field         | Type   | Description                                                  |
| ------------- | ------ | ------------------------------------------------------------ |
| size          | number | Number of transactions in the mempool                        |
| bytes         | number | Sum of virtual transaction sizes                             |
| usage         | number | Total memory usage of the mempool, in bytes                  |
| mempoolminfee | number | Minimum fee rate for a transaction to be accepted, in BTC/kB |
| minrelaytxfee | number | Minimum relay fee rate, in BTC/kB                            |

## Use Cases

* **Fee Estimation**: Read the current minimum fee rate
* **Congestion Monitoring**: Track mempool size and usage
* **Dashboards**: Display mempool state
* **Alerting**: Watch for mempool saturation

## Error Handling

| Error Code | Message           | Description                                  |
| ---------- | ----------------- | -------------------------------------------- |
| 403        | Forbidden         | Missing or invalid ACCESS-TOKEN              |
| -8         | Invalid parameter | A parameter is out of range or malformed     |
| -32601     | Method not found  | The method is not available on this endpoint |
| -32602     | Invalid params    | A parameter is missing or has the wrong type |
