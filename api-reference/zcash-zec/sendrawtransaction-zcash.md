---
description: >-
  Example code for the sendrawtransaction JSON-RPC method. Сomplete guide on how
  to use the sendrawtransaction JSON-RPC method in GetBlock.io Web3
  documentation.
---

# sendrawtransaction - Zcash

This method submits a signed raw transaction to the network. Zebrad has no wallet functionality, so applications must construct and sign transactions using an external wallet (Zallet, Ywallet, Zashi, or a library like `pyzcash`) before submission. Returns the transaction ID immediately if the transaction is accepted into the mempool.

## Parameters

| Parameter       | Type    | Required | Description                                                  |
| --------------- | ------- | -------- | ------------------------------------------------------------ |
| `hexstring`     | string  | Yes      | Hex-encoded signed serialized transaction                    |
| `allowhighfees` | boolean | No       | If `true`, bypass the high-fee sanity check. Default `false` |

## Request

{% tabs %}
{% tab title="cURL" %}
{% code overflow="wrap" %}
```bash
curl --location --request POST 'https://shared.eu-central-1.getblock.io/<ACCESS-TOKEN>/' \
--header 'Content-Type: application/json' \
--data-raw '{
    "jsonrpc": "2.0",
    "method": "sendrawtransaction",
    "params": [
        "0400008085202f89011234567890abcdef..."
    ],
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
    method: 'sendrawtransaction',
    params: [
        "0400008085202f89011234567890abcdef..."
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
        'method': 'sendrawtransaction',
        'params': [
        "0400008085202f89011234567890abcdef..."
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
            "method": "sendrawtransaction",
            "params": [
        "0400008085202f89011234567890abcdef..."
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
    "result": "9c8f0e4f8d2e7c6b5a4f3e2d1c0b9a8e7f6d5c4b3a2b1c9d8e7f6a5b4c3d2e1f"
}
```

## Response Parameters

| Parameter | Type   | Description                                         |
| --------- | ------ | --------------------------------------------------- |
| `result`  | string | 32-byte transaction ID of the submitted transaction |

## Use Cases

* **Wallet Transaction Submission**: Submit locally-signed transactions from Zallet, Ywallet, or Zashi to the network
* **Bridge Relayer Submission**: Cross-chain bridges submit signed transactions relaying messages onto Zcash
* **Backend Automation**: Custody systems using external HSM-backed signers submit signed transactions programmatically
* **Test Environment Broadcasts**: Submit test-signed transactions to Testnet during application development

## Error Handling

| Error Code | Message                            | Description                                                                      |
| ---------- | ---------------------------------- | -------------------------------------------------------------------------------- |
| -22        | TX decode failed                   | Transaction hex is malformed or the byte structure is invalid                    |
| -25        | TX rejected                        | Transaction rejected by consensus rules (invalid script, insufficient fee, etc.) |
| -26        | TX not accepted                    | Transaction rejected by mempool policy (e.g. non-standard, low fee, duplicate)   |
| -27        | Transaction already in block chain | This transaction was already mined                                               |
| -32602     | Invalid params                     | Hex string is malformed                                                          |
