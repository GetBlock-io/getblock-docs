---
description: >-
  Example code for the iota_getTransactionBlock JSON-RPC method. Complete guide
  on how to use iota_getTransactionBlock JSON-RPC method in GetBlock Web3
  documentation.
---

# iota\_getTransactionBlock - IOTA

Returns a transaction block by digest, with selectable fields: input, effects, events, object changes, and balance changes.

## Parameters

| Parameter | Type   | Required | Description                                                               |
| --------- | ------ | -------- | ------------------------------------------------------------------------- |
| digest    | string | Yes      | Transaction block digest (base58)                                         |
| options   | object | Optional | showInput, showEffects, showEvents, showObjectChanges, showBalanceChanges |

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
    "method": "iota_getTransactionBlock",
    "params": ["AdE...", {"showEffects": true, "showEvents": true, "showBalanceChanges": true}]
}'
```
{% endcode %}
{% endtab %}

{% tab title="Axios" %}
{% code title="example.js" %}
```javascript
const axios = require('axios');
const response = await axios.post('https://shared.eu-central-1.getblock.io/<ACCESS-TOKEN>/', { jsonrpc: '2.0', id: 'getblock.io', method: 'iota_getTransactionBlock', params: ["AdE...", {"showEffects": true, "showEvents": true, "showBalanceChanges": true}] }, { headers: { 'Content-Type': 'application/json' } });
console.log(response.data.result);
```
{% endcode %}
{% endtab %}

{% tab title="Request" %}
{% code title="example.py" %}
```python
import requests
response = requests.post('https://shared.eu-central-1.getblock.io/<ACCESS-TOKEN>/', headers={'Content-Type': 'application/json'}, json={'jsonrpc': '2.0', 'id': 'getblock.io', 'method': 'iota_getTransactionBlock', 'params': ["AdE...", {"showEffects": true, "showEvents": true, "showBalanceChanges": true}]})
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
    let res = client.post("https://shared.eu-central-1.getblock.io/<ACCESS-TOKEN>/").json(&json!({"jsonrpc":"2.0","id":"getblock.io","method":"iota_getTransactionBlock","params":["AdE...", {"showEffects": true, "showEvents": true, "showBalanceChanges": true}]})).send().await?.json::<Value>().await?;
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
        "digest": "AdE...",
        "checkpoint": "128344560",
        "timestampMs": "1736771990000",
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
                "owner": {
                    "AddressOwner": "0xabc..."
                },
                "coinType": "0x2::iota::IOTA",
                "amount": "-769760"
            }
        ]
    }
}
```

## Response Parameters

| Field                  | Type   | Description                            |
| ---------------------- | ------ | -------------------------------------- |
| jsonrpc                | string | JSON-RPC version ("2.0")               |
| id                     | string | Request identifier ("getblock.io")     |
| result.digest          | string | Transaction digest                     |
| result.effects.status  | object | Execution status (success/failure)     |
| result.effects.gasUsed | object | Computation, storage, and rebate costs |
| result.balanceChanges  | array  | Net balance deltas per owner/coin      |

## Use Cases

* **Receipt Lookup**: Confirm a transaction and read its effects
* **Accounting**: Read balance and object changes

## Error Handling

| Error                     | Message          | Description                                       |
| ------------------------- | ---------------- | ------------------------------------------------- |
| -32602                    | Invalid params   | A parameter was missing or malformed              |
| -32601                    | Method not found | The method name is misspelled or not served       |
| 403 / RBAC: access denied | Access denied    | The GetBlock access token is missing or incorrect |
