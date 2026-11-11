---
description: >-
  Example code for the getmempoolinfo JSON-RPC method. Complete guide on how to use
  getmempoolinfo JSON-RPC in GetBlock Web3 documentation.
---

# getmempoolinfo - Dogecoin

This method returns details on the active state of the transaction memory pool.

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
const { data } = await axios.post('https://shared.eu-central-1.getblock.io/<ACCESS-TOKEN>/', {
  jsonrpc: '2.0', method: 'getmempoolinfo', params: [], id: 'getblock.io'
});
console.log(data.result);
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
    "error": null,
    "id": "getblock.io",
    "result": {
        "size": 40,
        "bytes": 12874,
        "usage": 64720,
        "maxmempool": 300000000,
        "mempoolminfee": 0.01000000,
        "minrelaytxfee": 0.01000000
    }
}
```

## Response Parameters

| Parameter | Type         | Description                                      |
| --------- | ------------ | ------------------------------------------------ |
| error     | null\|object | Error object when the call fails, otherwise null |
| id        | string       | Request identifier matching the request          |
| result.size | numeric | Number of transactions in the mempool |
| result.bytes | numeric | Total serialized size of mempool transactions |
| result.usage | numeric | Total memory usage of the mempool in bytes |
| result.maxmempool | numeric | Maximum memory the mempool may use, in bytes |
| result.mempoolminfee | numeric | Minimum fee rate for acceptance, in DOGE per kilobyte |
| result.minrelaytxfee | numeric | Minimum relay fee rate, in DOGE per kilobyte |

{% hint style="info" %}
Dogecoin's relay minimum is high compared with other UTXO chains: `0.01` DOGE per kilobyte, where Bitcoin's default is `0.00001` BTC. A fee sized from a Bitcoin-derived default will be rejected.
{% endhint %}

## Use Cases

* **Congestion Checks**: Read mempool size before choosing a fee
* **Fee Floors**: Read `mempoolminfee` to avoid a rejected broadcast
* **Capacity Monitoring**: Compare `usage` against `maxmempool`

## Error Handling

| Error Code | Message | Description |
| ---------- | ------- | ----------- |
| -32700 | Parse error | Request body is not valid JSON |
| -32600 | Invalid request | The JSON sent is not a valid request object |
| -32603 | Internal error | Node failed to read the requested chain state |
