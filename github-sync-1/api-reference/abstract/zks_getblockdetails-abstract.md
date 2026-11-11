---
description: >-
  Example code for the zks_getBlockDetails JSON-RPC method. Complete guide on
  how to use zks_getBlockDetails JSON-RPC in GetBlock Web3 documentation.
---

# zks\_getBlockDetails - Abstract

Returns Abstract-specific details for an L2 block: its L1 batch number, timestamp, operator address, protocol version, and the L1 transaction hashes that committed and executed its batch.

## Parameters

| Parameter | Type    | Required | Description     |
| --------- | ------- | -------- | --------------- |
| block     | integer | Yes      | L2 block number |

## Request

{% tabs %}
{% tab title="cURL" %}
{% code overflow="wrap" %}
```bash
curl --location --request POST 'https://shared.eu-central-1.getblock.io/<ACCESS-TOKEN>/' \
--header 'Content-Type: application/json' \
--data-raw '{
    "jsonrpc": "2.0",
    "method": "zks_getBlockDetails",
    "params": [10000],
    "id": "getblock.io"
}'
```
{% endcode %}
{% endtab %}

{% tab title="Axios" %}
{% code title="example.js" %}
```javascript
const axios = require('axios');
const response = await axios.post('https://shared.eu-central-1.getblock.io/<ACCESS-TOKEN>/', { jsonrpc: '2.0', method: 'zks_getBlockDetails', params: [10000], id: 'getblock.io' }, { headers: { 'Content-Type': 'application/json' } });
console.log(response.data.result);
```
{% endcode %}
{% endtab %}

{% tab title="Request" %}
{% code title="example.py" %}
```python
import requests
response = requests.post('https://shared.eu-central-1.getblock.io/<ACCESS-TOKEN>/', headers={'Content-Type': 'application/json'}, json={'jsonrpc': '2.0', 'method': 'zks_getBlockDetails', 'params': [10000], 'id': 'getblock.io'})
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
    let res = client.post("https://shared.eu-central-1.getblock.io/<ACCESS-TOKEN>/").json(&json!({"jsonrpc":"2.0","method":"zks_getBlockDetails","params":[10000],"id":"getblock.io"})).send().await?.json::<Value>().await?;
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
        "number": 10000,
        "l1BatchNumber": 1000,
        "timestamp": 1730000000,
        "status": "verified",
        "operatorAddress": "0xd8dA6BF26964aF9D7eEd9e03E53415D37aA96045",
        "protocolVersion": "Version24",
        "commitTxHash": "0x6c0f1d0d6b9f6a2e3c4b8a7f5e9d0c1a3b5f7e9d2c4a6b8f0e1d3c5a7b9f1e2d",
        "executeTxHash": "0x6c0f1d0d6b9f6a2e3c4b8a7f5e9d0c1a3b5f7e9d2c4a6b8f0e1d3c5a7b9f1e2d"
    }
}
```

## Response Fields

| Field           | Type    | Description                   |
| --------------- | ------- | ----------------------------- |
| l1BatchNumber   | integer | L1 batch the block belongs to |
| status          | string  | Block settlement status       |
| protocolVersion | string  | Protocol version at the block |

## Use Cases

* **Settlement Status**: Check whether a block is verified on L1
* **Explorers**: Show L1 batch context for a block

## Error Handling

| Error                     | Message        | Description                                       |
| ------------------------- | -------------- | ------------------------------------------------- |
| -32602 / Invalid params   | Invalid params | A parameter was malformed                         |
| 403 / RBAC: access denied | Access denied  | The GetBlock access token is missing or incorrect |
