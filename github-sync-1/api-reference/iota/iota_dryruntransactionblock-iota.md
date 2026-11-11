---
description: >-
  Example code for the iota_dryRunTransactionBlock JSON-RPC method. Complete
  guide on how to use iota_dryRunTransactionBlock JSON-RPC method in GetBlock
  Web3 documentation.
---

# iota\_dryRunTransactionBlock - IOTA

Dry-runs a transaction block against the current state and returns its predicted effects and gas cost, without committing it. Use it to size gas before signing.

## Parameters

| Parameter | Type   | Required | Description                            |
| --------- | ------ | -------- | -------------------------------------- |
| tx\_bytes | string | Yes      | Base64 BCS-serialized transaction data |

## Request

{% tabs %}
{% tab title="cURL" %}
{% code overflow="wrap" %}
```bash
curl --location --request POST 'https://shared.eu-central-1.getblock.io/<ACCESS-TOKEN>/' \
--header 'Content-Type: application/json' \
--data-raw '{
    "jsonrpc": "2.0",
    "id": "getblock.io",
    "method": "iota_dryRunTransactionBlock",
    "params": ["AAACAQD..."]
}'
```
{% endcode %}
{% endtab %}

{% tab title="Axios" %}
{% code title="example.js" %}
```javascript
const axios = require('axios');
const response = await axios.post('https://shared.eu-central-1.getblock.io/<ACCESS-TOKEN>/', { jsonrpc: '2.0', id: 'getblock.io', method: 'iota_dryRunTransactionBlock', params: ["AAACAQD..."] }, { headers: { 'Content-Type': 'application/json' } });
console.log(response.data.result);
```
{% endcode %}
{% endtab %}

{% tab title="Request" %}
{% code title="example.py" %}
```python
import requests
response = requests.post('https://shared.eu-central-1.getblock.io/<ACCESS-TOKEN>/', headers={'Content-Type': 'application/json'}, json={'jsonrpc': '2.0', 'id': 'getblock.io', 'method': 'iota_dryRunTransactionBlock', 'params': ["AAACAQD..."]})
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
    let res = client.post("https://shared.eu-central-1.getblock.io/<ACCESS-TOKEN>/").json(&json!({"jsonrpc":"2.0","id":"getblock.io","method":"iota_dryRunTransactionBlock","params":["AAACAQD..."]})).send().await?.json::<Value>().await?;
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
        "effects": {
            "status": {
                "status": "success"
            },
            "gasUsed": {
                "computationCost": "750000",
                "storageCost": "1976000",
                "storageRebate": "1956240"
            }
        },
        "balanceChanges": [
            {
                "coinType": "0x2::iota::IOTA",
                "amount": "-769760"
            }
        ]
    }
}
```

## Response Parameters

| Field                 | Type   | Description                        |
| --------------------- | ------ | ---------------------------------- |
| jsonrpc               | string | JSON-RPC version ("2.0")           |
| id                    | string | Request identifier ("getblock.io") |
| result.effects        | object | Predicted execution effects        |
| result.balanceChanges | array  | Predicted balance deltas           |

## Use Cases

* **Gas Estimation**: Size gas before submitting
* **Validation**: Catch failures before broadcasting

## Error Handling

| Error                     | Message          | Description                                       |
| ------------------------- | ---------------- | ------------------------------------------------- |
| -32602                    | Invalid params   | A parameter was missing or malformed              |
| -32601                    | Method not found | The method name is misspelled or not served       |
| 403 / RBAC: access denied | Access denied    | The GetBlock access token is missing or incorrect |
