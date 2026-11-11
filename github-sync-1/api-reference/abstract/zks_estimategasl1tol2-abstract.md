---
description: >-
  Example code for the zks_estimateGasL1ToL2 JSON-RPC method. Complete guide on
  how to use zks_estimateGasL1ToL2 JSON-RPC in GetBlock Web3 documentation.
---

# zks\_estimateGasL1ToL2 - Abstract

Estimates the L2 gas required to execute a transaction sent from Ethereum L1 to Abstract, used when building deposits and L1-triggered calls.

## Parameters

| Parameter | Type   | Required | Description                 |
| --------- | ------ | -------- | --------------------------- |
| request   | object | Yes      | Transaction request from L1 |

## Request

{% tabs %}
{% tab title="cURL" %}
{% code overflow="wrap" %}
```bash
curl --location --request POST 'https://shared.eu-central-1.getblock.io/<ACCESS-TOKEN>/' \
--header 'Content-Type: application/json' \
--data-raw '{
    "jsonrpc": "2.0",
    "method": "zks_estimateGasL1ToL2",
    "params": [{"from": "0xd8dA6BF26964aF9D7eEd9e03E53415D37aA96045", "to": "0xd8dA6BF26964aF9D7eEd9e03E53415D37aA96045", "value": "0xde0b6b3a7640000"}],
    "id": "getblock.io"
}'
```
{% endcode %}
{% endtab %}

{% tab title="Axios" %}
{% code title="example.js" %}
```javascript
const axios = require('axios');
const response = await axios.post('https://shared.eu-central-1.getblock.io/<ACCESS-TOKEN>/', { jsonrpc: '2.0', method: 'zks_estimateGasL1ToL2', params: [{"from": "0xd8dA6BF26964aF9D7eEd9e03E53415D37aA96045", "to": "0xd8dA6BF26964aF9D7eEd9e03E53415D37aA96045", "value": "0xde0b6b3a7640000"}], id: 'getblock.io' }, { headers: { 'Content-Type': 'application/json' } });
console.log(response.data.result);
```
{% endcode %}
{% endtab %}

{% tab title="Request" %}
{% code title="example.py" %}
```python
import requests
response = requests.post('https://shared.eu-central-1.getblock.io/<ACCESS-TOKEN>/', headers={'Content-Type': 'application/json'}, json={'jsonrpc': '2.0', 'method': 'zks_estimateGasL1ToL2', 'params': [{"from": "0xd8dA6BF26964aF9D7eEd9e03E53415D37aA96045", "to": "0xd8dA6BF26964aF9D7eEd9e03E53415D37aA96045", "value": "0xde0b6b3a7640000"}], 'id': 'getblock.io'})
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
    let res = client.post("https://shared.eu-central-1.getblock.io/<ACCESS-TOKEN>/").json(&json!({"jsonrpc":"2.0","method":"zks_estimateGasL1ToL2","params":[{"from": "0xd8dA6BF26964aF9D7eEd9e03E53415D37aA96045", "to": "0xd8dA6BF26964aF9D7eEd9e03E53415D37aA96045", "value": "0xde0b6b3a7640000"}],"id":"getblock.io"})).send().await?.json::<Value>().await?;
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
    "result": "0x30d40"
}
```

## Response Fields

| Field  | Type   | Description            |
| ------ | ------ | ---------------------- |
| result | string | Estimated L2 gas (hex) |

## Use Cases

* **Bridging**: Size L1-to-L2 deposits
* **Cross-Layer Calls**: Estimate L1-triggered execution

## Error Handling

| Error                     | Message        | Description                                       |
| ------------------------- | -------------- | ------------------------------------------------- |
| -32602 / Invalid params   | Invalid params | A parameter was malformed                         |
| 403 / RBAC: access denied | Access denied  | The GetBlock access token is missing or incorrect |
