---
description: >-
  Example code for the iota_devInspectTransactionBlock JSON-RPC method. Complete
  guide on how to use iota_devInspectTransactionBlock JSON-RPC method in
  GetBlock Web3 documentation.
---

# iota\_devInspectTransactionBlock - IOTA

Simulates a transaction from its components for inspection, returning execution results including Move return values — without needing valid gas or signatures. Ideal for read-only Move calls.

## Parameters

| Parameter       | Type    | Required | Description                              |
| --------------- | ------- | -------- | ---------------------------------------- |
| sender\_address | string  | Yes      | Address to simulate the transaction from |
| tx\_bytes       | string  | Yes      | Base64 BCS transaction kind              |
| gas\_price      | integer | Optional | Gas price to simulate with               |
| epoch           | integer | Optional | Epoch to simulate at                     |

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
    "method": "iota_devInspectTransactionBlock",
    "params": ["0xabc...", "AAACAQD...", null, null]
}'
```
{% endcode %}
{% endtab %}

{% tab title="Axios" %}
{% code title="example.js" %}
```javascript
const axios = require('axios');
const response = await axios.post('https://shared.eu-central-1.getblock.io/<ACCESS-TOKEN>/', { jsonrpc: '2.0', id: 'getblock.io', method: 'iota_devInspectTransactionBlock', params: ["0xabc...", "AAACAQD...", null, null] }, { headers: { 'Content-Type': 'application/json' } });
console.log(response.data.result);
```
{% endcode %}
{% endtab %}

{% tab title="Request" %}
{% code title="example.py" %}
```python
import requests
response = requests.post('https://shared.eu-central-1.getblock.io/<ACCESS-TOKEN>/', headers={'Content-Type': 'application/json'}, json={'jsonrpc': '2.0', 'id': 'getblock.io', 'method': 'iota_devInspectTransactionBlock', 'params': ["0xabc...", "AAACAQD...", null, null]})
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
    let res = client.post("https://shared.eu-central-1.getblock.io/<ACCESS-TOKEN>/").json(&json!({"jsonrpc":"2.0","id":"getblock.io","method":"iota_devInspectTransactionBlock","params":["0xabc...", "AAACAQD...", null, null]})).send().await?.json::<Value>().await?;
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
            }
        },
        "results": [
            {
                "returnValues": [
                    [
                        [
                            1,
                            0,
                            0,
                            0
                        ],
                        "u64"
                    ]
                ]
            }
        ]
    }
}
```

## Response Parameters

| Field          | Type   | Description                              |
| -------------- | ------ | ---------------------------------------- |
| jsonrpc        | string | JSON-RPC version ("2.0")                 |
| id             | string | Request identifier ("getblock.io")       |
| result.effects | object | Simulated effects                        |
| result.results | array  | Per-command return values and Move types |

## Use Cases

* **Read-Only Calls**: Execute a Move function to read state
* **Inspection**: Preview return values without a transaction

## Error Handling

| Error                     | Message          | Description                                       |
| ------------------------- | ---------------- | ------------------------------------------------- |
| -32602                    | Invalid params   | A parameter was missing or malformed              |
| -32601                    | Method not found | The method name is misspelled or not served       |
| 403 / RBAC: access denied | Access denied    | The GetBlock access token is missing or incorrect |
