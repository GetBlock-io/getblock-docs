---
description: >-
  Example code for the unsafe_batchTransaction JSON-RPC method. Complete guide
  on how to use unsafe_batchTransaction JSON-RPC method in GetBlock Web3
  documentation.
---

# unsafe\_batchTransaction - IOTA

Builds an unsigned transaction that batches several operations (transfers, Move calls) into one atomic transaction.

## Parameters

| Parameter                   | Type   | Required | Description                    |
| --------------------------- | ------ | -------- | ------------------------------ |
| signer                      | string | Yes      | Sender address                 |
| single\_transaction\_params | array  | Yes      | Array of operation descriptors |
| gas                         | string | Optional | Gas coin object ID             |
| gas\_budget                 | string | Yes      | Gas budget                     |

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
    "method": "unsafe_batchTransaction",
    "params": ["0xabc...", [{"transferObjectRequestParams": {"objectId": "0x77aa...", "recipient": "0xdef..."}}], null, "10000000"]
}'
```
{% endcode %}
{% endtab %}

{% tab title="Axios" %}
{% code title="example.js" %}
```javascript
const axios = require('axios');
const response = await axios.post('https://shared.eu-central-1.getblock.io/<ACCESS-TOKEN>/', { jsonrpc: '2.0', id: 'getblock.io', method: 'unsafe_batchTransaction', params: ["0xabc...", [{"transferObjectRequestParams": {"objectId": "0x77aa...", "recipient": "0xdef..."}}], null, "10000000"] }, { headers: { 'Content-Type': 'application/json' } });
console.log(response.data.result);
```
{% endcode %}
{% endtab %}

{% tab title="Request" %}
{% code title="example.py" %}
```python
import requests
response = requests.post('https://shared.eu-central-1.getblock.io/<ACCESS-TOKEN>/', headers={'Content-Type': 'application/json'}, json={'jsonrpc': '2.0', 'id': 'getblock.io', 'method': 'unsafe_batchTransaction', 'params': ["0xabc...", [{"transferObjectRequestParams": {"objectId": "0x77aa...", "recipient": "0xdef..."}}], null, "10000000"]})
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
    let res = client.post("https://shared.eu-central-1.getblock.io/<ACCESS-TOKEN>/").json(&json!({"jsonrpc":"2.0","id":"getblock.io","method":"unsafe_batchTransaction","params":["0xabc...", [{"transferObjectRequestParams": {"objectId": "0x77aa...", "recipient": "0xdef..."}}], null, "10000000"]})).send().await?.json::<Value>().await?;
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
        "txBytes": "AAACAQD..."
    }
}
```

## Response Parameters

| Field          | Type   | Description                          |
| -------------- | ------ | ------------------------------------ |
| jsonrpc        | string | JSON-RPC version ("2.0")             |
| id             | string | Request identifier ("getblock.io")   |
| result.txBytes | string | Base64 BCS transaction bytes to sign |

## Use Cases

* **Atomic Ops**: Combine multiple actions in one transaction

## Error Handling

| Error                     | Message          | Description                                       |
| ------------------------- | ---------------- | ------------------------------------------------- |
| -32602                    | Invalid params   | A parameter was missing or malformed              |
| -32601                    | Method not found | The method name is misspelled or not served       |
| 403 / RBAC: access denied | Access denied    | The GetBlock access token is missing or incorrect |
