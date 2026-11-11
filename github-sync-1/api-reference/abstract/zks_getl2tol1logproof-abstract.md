---
description: >-
  Example code for the zks_getL2ToL1LogProof JSON-RPC method. Complete guide on
  how to use zks_getL2ToL1LogProof JSON-RPC in GetBlock Web3 documentation.
---

# zks\_getL2ToL1LogProof - Abstract

Returns the Merkle proof for an L2-to-L1 log emitted by a transaction, required to finalize a withdrawal or message on Ethereum L1.

## Parameters

| Parameter | Type    | Required | Description                                  |
| --------- | ------- | -------- | -------------------------------------------- |
| txHash    | string  | Yes      | Hash of the L2 transaction                   |
| index     | integer | Optional | Index of the L2-to-L1 log in the transaction |

## Request

{% tabs %}
{% tab title="cURL" %}
{% code overflow="wrap" %}
```bash
curl --location --request POST 'https://shared.eu-central-1.getblock.io/<ACCESS-TOKEN>/' \
--header 'Content-Type: application/json' \
--data-raw '{
    "jsonrpc": "2.0",
    "method": "zks_getL2ToL1LogProof",
    "params": ["0x6c0f1d0d6b9f6a2e3c4b8a7f5e9d0c1a3b5f7e9d2c4a6b8f0e1d3c5a7b9f1e2d", 0],
    "id": "getblock.io"
}'
```
{% endcode %}
{% endtab %}

{% tab title="Axios" %}
{% code title="example.js" %}
```javascript
const axios = require('axios');
const response = await axios.post('https://shared.eu-central-1.getblock.io/<ACCESS-TOKEN>/', { jsonrpc: '2.0', method: 'zks_getL2ToL1LogProof', params: ["0x6c0f1d0d6b9f6a2e3c4b8a7f5e9d0c1a3b5f7e9d2c4a6b8f0e1d3c5a7b9f1e2d", 0], id: 'getblock.io' }, { headers: { 'Content-Type': 'application/json' } });
console.log(response.data.result);
```
{% endcode %}
{% endtab %}

{% tab title="Request" %}
{% code title="example.py" %}
```python
import requests
response = requests.post('https://shared.eu-central-1.getblock.io/<ACCESS-TOKEN>/', headers={'Content-Type': 'application/json'}, json={'jsonrpc': '2.0', 'method': 'zks_getL2ToL1LogProof', 'params': ["0x6c0f1d0d6b9f6a2e3c4b8a7f5e9d0c1a3b5f7e9d2c4a6b8f0e1d3c5a7b9f1e2d", 0], 'id': 'getblock.io'})
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
    let res = client.post("https://shared.eu-central-1.getblock.io/<ACCESS-TOKEN>/").json(&json!({"jsonrpc":"2.0","method":"zks_getL2ToL1LogProof","params":["0x6c0f1d0d6b9f6a2e3c4b8a7f5e9d0c1a3b5f7e9d2c4a6b8f0e1d3c5a7b9f1e2d", 0],"id":"getblock.io"})).send().await?.json::<Value>().await?;
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
        "id": 0,
        "proof": [
            "0x...",
            "0x..."
        ],
        "root": "0x..."
    }
}
```

## Response Fields

| Field | Type    | Description                       |
| ----- | ------- | --------------------------------- |
| proof | array   | Merkle proof for the L2-to-L1 log |
| id    | integer | Log index                         |
| root  | string  | Merkle root                       |

## Use Cases

* **Withdrawals**: Finalize a withdrawal on L1
* **Messaging**: Prove an L2-to-L1 message

## Error Handling

| Error                     | Message        | Description                                       |
| ------------------------- | -------------- | ------------------------------------------------- |
| -32602 / Invalid params   | Invalid params | A parameter was malformed                         |
| 403 / RBAC: access denied | Access denied  | The GetBlock access token is missing or incorrect |
