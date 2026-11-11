---
description: >-
  Example code for the zks_getL1BatchDetails JSON-RPC method. Complete guide on
  how to use zks_getL1BatchDetails JSON-RPC in GetBlock Web3 documentation.
---

# zks\_getL1BatchDetails - Abstract

Returns metadata for an L1 batch: its status, the L1 commit/prove/execute transaction hashes, timestamp, and root hash. Used to track a batch through the ZK settlement lifecycle.

## Parameters

| Parameter   | Type    | Required | Description     |
| ----------- | ------- | -------- | --------------- |
| batchNumber | integer | Yes      | L1 batch number |

## Request

{% tabs %}
{% tab title="cURL" %}
{% code overflow="wrap" %}
```bash
curl --location --request POST 'https://shared.eu-central-1.getblock.io/<ACCESS-TOKEN>/' \
--header 'Content-Type: application/json' \
--data-raw '{
    "jsonrpc": "2.0",
    "method": "zks_getL1BatchDetails",
    "params": [1000],
    "id": "getblock.io"
}'
```
{% endcode %}
{% endtab %}

{% tab title="Axios" %}
{% code title="example.js" %}
```javascript
const axios = require('axios');
const response = await axios.post('https://shared.eu-central-1.getblock.io/<ACCESS-TOKEN>/', { jsonrpc: '2.0', method: 'zks_getL1BatchDetails', params: [1000], id: 'getblock.io' }, { headers: { 'Content-Type': 'application/json' } });
console.log(response.data.result);
```
{% endcode %}
{% endtab %}

{% tab title="Request" %}
{% code title="example.py" %}
```python
import requests
response = requests.post('https://shared.eu-central-1.getblock.io/<ACCESS-TOKEN>/', headers={'Content-Type': 'application/json'}, json={'jsonrpc': '2.0', 'method': 'zks_getL1BatchDetails', 'params': [1000], 'id': 'getblock.io'})
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
    let res = client.post("https://shared.eu-central-1.getblock.io/<ACCESS-TOKEN>/").json(&json!({"jsonrpc":"2.0","method":"zks_getL1BatchDetails","params":[1000],"id":"getblock.io"})).send().await?.json::<Value>().await?;
    println!("{}", res["result"]);
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
    "number": 1000,
    "status": "verified",
    "commitTxHash": "0x6c0f1d0d6b9f6a2e3c4b8a7f5e9d0c1a3b5f7e9d2c4a6b8f0e1d3c5a7b9f1e2d",
    "proveTxHash": "0x6c0f1d0d6b9f6a2e3c4b8a7f5e9d0c1a3b5f7e9d2c4a6b8f0e1d3c5a7b9f1e2d",
    "executeTxHash": "0x6c0f1d0d6b9f6a2e3c4b8a7f5e9d0c1a3b5f7e9d2c4a6b8f0e1d3c5a7b9f1e2d",
    "timestamp": 1730000000,
    "l2TxCount": 250,
    "rootHash": "0x..."
  }
}
```

## Response Fields

| Field         | Type   | Description                                        |
| ------------- | ------ | -------------------------------------------------- |
| status        | string | Batch status (sealed, committed, proven, verified) |
| commitTxHash  | string | L1 tx that committed the batch                     |
| executeTxHash | string | L1 tx that finalized the batch                     |

## Use Cases

* **Settlement Tracking**: Follow a batch to L1 finality
* **Bridges**: Confirm a batch is verified before releasing funds

## Error Handling

| Error                     | Message        | Description                                       |
| ------------------------- | -------------- | ------------------------------------------------- |
| -32602 / Invalid params   | Invalid params | A parameter was malformed                         |
| 403 / RBAC: access denied | Access denied  | The GetBlock access token is missing or incorrect |
