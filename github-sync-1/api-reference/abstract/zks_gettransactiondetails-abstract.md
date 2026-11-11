---
description: >-
  Example code for the zks_getTransactionDetails JSON-RPC method. Complete guide
  on how to use zks_getTransactionDetails JSON-RPC in GetBlock Web3
  documentation.
---

# zks\_getTransactionDetails - Abstract

Returns Abstract-specific details for a transaction: whether it originated on L1, its status, the fee paid, the initiator, and the gas-per-pubdata limit used by the ZK fee model.

## Parameters

| Parameter | Type   | Required | Description      |
| --------- | ------ | -------- | ---------------- |
| txHash    | string | Yes      | Transaction hash |

## Request

{% tabs %}
{% tab title="cURL" %}
{% code overflow="wrap" %}
```bash
curl --location --request POST 'https://shared.eu-central-1.getblock.io/<ACCESS-TOKEN>/' \
--header 'Content-Type: application/json' \
--data-raw '{
    "jsonrpc": "2.0",
    "method": "zks_getTransactionDetails",
    "params": ["0x6c0f1d0d6b9f6a2e3c4b8a7f5e9d0c1a3b5f7e9d2c4a6b8f0e1d3c5a7b9f1e2d"],
    "id": "getblock.io"
}'
```
{% endcode %}
{% endtab %}

{% tab title="Axios" %}
{% code title="example.js" %}
```javascript
const axios = require('axios');
const response = await axios.post('https://shared.eu-central-1.getblock.io/<ACCESS-TOKEN>/', { jsonrpc: '2.0', method: 'zks_getTransactionDetails', params: ["0x6c0f1d0d6b9f6a2e3c4b8a7f5e9d0c1a3b5f7e9d2c4a6b8f0e1d3c5a7b9f1e2d"], id: 'getblock.io' }, { headers: { 'Content-Type': 'application/json' } });
console.log(response.data.result);
```
{% endcode %}
{% endtab %}

{% tab title="Request" %}
{% code title="example.py" %}
```python
import requests
response = requests.post('https://shared.eu-central-1.getblock.io/<ACCESS-TOKEN>/', headers={'Content-Type': 'application/json'}, json={'jsonrpc': '2.0', 'method': 'zks_getTransactionDetails', 'params': ["0x6c0f1d0d6b9f6a2e3c4b8a7f5e9d0c1a3b5f7e9d2c4a6b8f0e1d3c5a7b9f1e2d"], 'id': 'getblock.io'})
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
    let res = client.post("https://shared.eu-central-1.getblock.io/<ACCESS-TOKEN>/").json(&json!({"jsonrpc":"2.0","method":"zks_getTransactionDetails","params":["0x6c0f1d0d6b9f6a2e3c4b8a7f5e9d0c1a3b5f7e9d2c4a6b8f0e1d3c5a7b9f1e2d"],"id":"getblock.io"})).send().await?.json::<Value>().await?;
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
        "isL1Originated": false,
        "status": "verified",
        "fee": "0x1a2b3c",
        "initiatorAddress": "0xd8dA6BF26964aF9D7eEd9e03E53415D37aA96045",
        "receivedAt": "2025-11-01T12:00:00Z",
        "gasPerPubdata": "0x320"
    }
}
```

## Response Fields

| Field          | Type    | Description                            |
| -------------- | ------- | -------------------------------------- |
| isL1Originated | boolean | Whether the tx came from an L1 deposit |
| status         | string  | Settlement status                      |
| fee            | string  | Fee paid in wei                        |
| gasPerPubdata  | string  | Gas per pubdata byte used              |

## Use Cases

* **Status Polling**: Track a tx to L1 finality
* **Fee Accounting**: Read the exact fee and pubdata cost

## Error Handling

| Error                     | Message        | Description                                       |
| ------------------------- | -------------- | ------------------------------------------------- |
| -32602 / Invalid params   | Invalid params | A parameter was malformed                         |
| 403 / RBAC: access denied | Access denied  | The GetBlock access token is missing or incorrect |
