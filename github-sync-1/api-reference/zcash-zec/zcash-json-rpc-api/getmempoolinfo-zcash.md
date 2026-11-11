---
description: >-
  Example code for the getmempoolinfo JSON-RPC method. Complete guide on how to
  use the getmempoolinfo JSON-RPC method in GetBlock.io Web3 documentation.
---

# getmempoolinfo - Zcash

This method returns aggregate mempool statistics: the number of transactions queued and the memory they occupy.

{% hint style="info" %}
Zebra returns a smaller object than Bitcoin Core's `getmempoolinfo`. The fee-threshold fields `mempoolminfee` and `minrelaytxfee`, and the `maxmempool` limit, are not present, so a client ported from Bitcoin Core must not depend on them.
{% endhint %}

## Parameters

This method takes no parameters.

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

{% tab title="Axios" %}
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
    "result": {
        "size": 7,
        "bytes": 55446,
        "usage": 55446
    },
    "id": "getblock.io"
}
```

## Response Parameters

| Parameter       | Type    | Description                                            |
| --------------- | ------- | ------------------------------------------------------ |
| `size`          | integer | Number of transactions in the mempool                  |
| `bytes`         | integer | Total serialized size of mempool transactions in bytes |
| `usage`         | integer | Total memory usage of the mempool in bytes             |

## Use Cases

* **Mempool Health Monitoring**: Alert when `size` or `bytes` approach configured limits
* **Fee Estimation Baselines**: Use `mempoolminfee` as a floor for constructing new transactions during congestion
* **Capacity Planning**: Track mempool memory usage on self-hosted nodes for resource sizing
* **Congestion Signals**: Compare `bytes` to `maxmempool` to detect capacity pressure

## Error Handling

| Error Code | Message        | Description                              |
| ---------- | -------------- | ---------------------------------------- |
| -32603     | Internal error | Node failed to compile the mempool stats |
