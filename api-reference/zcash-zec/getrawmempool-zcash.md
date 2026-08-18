---
description: >-
  Example code for the getrawmempool JSON-RPC method. Сomplete guide on how to
  use the getrawmempool JSON-RPC method in GetBlock.io Web3 documentation.
---

# getrawmempool - Zcash

This method returns all transaction IDs currently in the node's mempool. Verbose mode returns full transaction metadata (fees, size, times) instead of just IDs. Post-Zebra 3.0.0, the mempool index bug that caused unnecessary per-transaction rebuilds during `getrawmempool` calls has been fixed, meaningfully improving performance under load.

## Parameters

| Parameter | Type    | Required | Description                                                                                   |
| --------- | ------- | -------- | --------------------------------------------------------------------------------------------- |
| `verbose` | boolean | No       | If `true`, return full metadata per transaction; if `false`, return only IDs. Default `false` |

## Request

{% tabs %}
{% tab title="cURL" %}
```bash
curl --location --request POST 'https://shared.eu-central-1.getblock.io/<ACCESS-TOKEN>/' \
--header 'Content-Type: application/json' \
--data-raw '{
    "jsonrpc": "2.0",
    "method": "getrawmempool",
    "params": [
        false
    ],
    "id": "getblock.io"
}'
```
{% endtab %}

{% tab title="Axios" %}
{% code title="example.js" %}
```javascript
const axios = require('axios');

const response = await axios.post('https://shared.eu-central-1.getblock.io/<ACCESS-TOKEN>/', {
    jsonrpc: '2.0',
    method: 'getrawmempool',
    params: [
        false
    ],
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
        'method': 'getrawmempool',
        'params': [
        false
    ],
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
            "method": "getrawmempool",
            "params": [
        false
    ],
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
    "result": [
        "9c8f0e4f8d2e7c6b5a4f3e2d1c0b9a8e7f6d5c4b3a2b1c9d8e7f6a5b4c3d2e1f",
        "1cd7d1e5a67c7e4f1bb2d4c50a9c27c3b4a7f9e6b7a4919c2d5b0a6f3d9e1c4b"
    ]
}
```

## Response Parameters

| Parameter | Type                      | Description                                                                                           |
| --------- | ------------------------- | ----------------------------------------------------------------------------------------------------- |
| `result`  | array of string or object | Transaction IDs (verbose=false) or transaction detail objects (verbose=true) currently in the mempool |

## Use Cases

* **Mempool Monitoring**: Track transactions waiting for inclusion — feed data into MEV or activity dashboards
* **Post-Submit Tracking**: After `sendrawtransaction`, poll the mempool to confirm the transaction is queued
* **Congestion Detection**: Mempool size and composition indicate network congestion levels
* **Fee Estimation Input**: Analyze mempool fee distribution to estimate appropriate fees for new transactions

## Error Handling

| Error Code | Message        | Description                       |
| ---------- | -------------- | --------------------------------- |
| -32603     | Internal error | Node failed to read mempool state |
