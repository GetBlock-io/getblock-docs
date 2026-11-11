---
description: >-
  Example code for the unsafe_payIota JSON-RPC method. Complete guide on how to
  use unsafe_payIota JSON-RPC method in GetBlock Web3 documentation.
---

# unsafe\_payIota - IOTA

Builds an unsigned transaction that pays IOTA to recipients, using the input IOTA coins themselves to pay gas (no separate gas coin).

## Parameters

| Parameter    | Type   | Required | Description           |
| ------------ | ------ | -------- | --------------------- |
| signer       | string | Yes      | Sender address        |
| input\_coins | array  | Yes      | IOTA coin object IDs  |
| recipients   | array  | Yes      | Recipient addresses   |
| amounts      | array  | Yes      | Amounts per recipient |
| gas\_budget  | string | Yes      | Gas budget            |

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
    "method": "unsafe_payIota",
    "params": ["0xabc...", ["0x9f..."], ["0xdef..."], ["1000000000"], "5000000"]
}'
```
{% endcode %}
{% endtab %}

{% tab title="Axios" %}
{% code title="example.js" %}
```javascript
const axios = require('axios');
const response = await axios.post('https://shared.eu-central-1.getblock.io/<ACCESS-TOKEN>/', { jsonrpc: '2.0', id: 'getblock.io', method: 'unsafe_payIota', params: ["0xabc...", ["0x9f..."], ["0xdef..."], ["1000000000"], "5000000"] }, { headers: { 'Content-Type': 'application/json' } });
console.log(response.data.result);
```
{% endcode %}
{% endtab %}

{% tab title="Request" %}
{% code title="example.py" %}
```python
import requests
response = requests.post('https://shared.eu-central-1.getblock.io/<ACCESS-TOKEN>/', headers={'Content-Type': 'application/json'}, json={'jsonrpc': '2.0', 'id': 'getblock.io', 'method': 'unsafe_payIota', 'params': ["0xabc...", ["0x9f..."], ["0xdef..."], ["1000000000"], "5000000"]})
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
    let res = client.post("https://shared.eu-central-1.getblock.io/<ACCESS-TOKEN>/").json(&json!({"jsonrpc":"2.0","id":"getblock.io","method":"unsafe_payIota","params":["0xabc...", ["0x9f..."], ["0xdef..."], ["1000000000"], "5000000"]})).send().await?.json::<Value>().await?;
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

* **IOTA Transfers**: Pay IOTA without a dedicated gas coin

## Error Handling

| Error                     | Message          | Description                                       |
| ------------------------- | ---------------- | ------------------------------------------------- |
| -32602                    | Invalid params   | A parameter was missing or malformed              |
| -32601                    | Method not found | The method name is misspelled or not served       |
| 403 / RBAC: access denied | Access denied    | The GetBlock access token is missing or incorrect |
