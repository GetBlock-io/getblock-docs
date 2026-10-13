---
description: >-
  Example code for the iota_executeTransactionBlock JSON-RPC method. Complete
  guide on how to use iota_executeTransactionBlock JSON-RPC method in GetBlock
  Web3 documentation.
---

# iota\_executeTransactionBlock - IOTA

Submits a signed transaction block (BCS-serialized) with its signatures for execution, and returns the requested effects. This is how a transaction is broadcast to IOTA.

## Parameters

| Parameter     | Type   | Required | Description                                 |
| ------------- | ------ | -------- | ------------------------------------------- |
| tx\_bytes     | string | Yes      | Base64 BCS-serialized transaction data      |
| signatures    | array  | Yes      | Array of base64 signatures                  |
| options       | object | Optional | Response display options                    |
| request\_type | string | Optional | WaitForEffectsCert or WaitForLocalExecution |

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
    "method": "iota_executeTransactionBlock",
    "params": ["AAACAQD...", ["AN1s..."], {"showEffects": true}, "WaitForLocalExecution"]
}'
```
{% endcode %}
{% endtab %}

{% tab title="Axios" %}
{% code title="example.js" %}
```javascript
const axios = require('axios');
const response = await axios.post('https://shared.eu-central-1.getblock.io/<ACCESS-TOKEN>/', { jsonrpc: '2.0', id: 'getblock.io', method: 'iota_executeTransactionBlock', params: ["AAACAQD...", ["AN1s..."], {"showEffects": true}, "WaitForLocalExecution"] }, { headers: { 'Content-Type': 'application/json' } });
console.log(response.data.result);
```
{% endcode %}
{% endtab %}

{% tab title="Request" %}
{% code title="example.py" %}
```python
import requests
response = requests.post('https://shared.eu-central-1.getblock.io/<ACCESS-TOKEN>/', headers={'Content-Type': 'application/json'}, json={'jsonrpc': '2.0', 'id': 'getblock.io', 'method': 'iota_executeTransactionBlock', 'params': ["AAACAQD...", ["AN1s..."], {"showEffects": true}, "WaitForLocalExecution"]})
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
    let res = client.post("https://shared.eu-central-1.getblock.io/<ACCESS-TOKEN>/").json(&json!({"jsonrpc":"2.0","id":"getblock.io","method":"iota_executeTransactionBlock","params":["AAACAQD...", ["AN1s..."], {"showEffects": true}, "WaitForLocalExecution"]})).send().await?.json::<Value>().await?;
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
        "effects": {
            "status": {
                "status": "success"
            },
            "gasUsed": {
                "computationCost": "750000",
                "storageCost": "1976000",
                "storageRebate": "1956240"
            }
        }
    }
}
```

## Response Parameters

| Field          | Type   | Description                        |
| -------------- | ------ | ---------------------------------- |
| jsonrpc        | string | JSON-RPC version ("2.0")           |
| id             | string | Request identifier ("getblock.io") |
| result.digest  | string | Digest of the executed transaction |
| result.effects | object | Execution effects when requested   |

## Use Cases

* **Transaction Submission**: Broadcast a signed transaction
* **dApp Writes**: Transfer IOTA or call Move functions

## Error Handling

| Error                     | Message          | Description                                       |
| ------------------------- | ---------------- | ------------------------------------------------- |
| -32602                    | Invalid params   | A parameter was missing or malformed              |
| -32601                    | Method not found | The method name is misspelled or not served       |
| 403 / RBAC: access denied | Access denied    | The GetBlock access token is missing or incorrect |
