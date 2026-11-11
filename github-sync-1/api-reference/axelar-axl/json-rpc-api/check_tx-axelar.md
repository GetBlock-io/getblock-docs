---
description: >-
  Example code for the check_tx JSON-RPC method. Complete guide on how to use
  check_tx JSON-RPC in GetBlock Web3 documentation.
---

# check\_tx - Axelar

Runs CheckTx against a transaction (validating it against the mempool rules) and returns the result, without adding it to the mempool.

## Parameters

| Parameter | Type   | Required | Description              |
| --------- | ------ | -------- | ------------------------ |
| tx        | string | Yes      | Base64 transaction bytes |

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
    "method": "check_tx",
    "params": {"tx": "Cr0BC..."}
}'
```
{% endcode %}
{% endtab %}

{% tab title="Axios" %}
{% code title="example.js" %}
```javascript
const axios = require('axios');
const response = await axios.post('https://shared.eu-central-1.getblock.io/<ACCESS-TOKEN>/', { jsonrpc: '2.0', id: 'getblock.io', method: 'check_tx', params: {"tx": "Cr0BC..."} }, { headers: { 'Content-Type': 'application/json' } });
console.log(response.data.result);
```
{% endcode %}
{% endtab %}

{% tab title="Request" %}
{% code title="example.py" %}
```python
import requests
response = requests.post('https://shared.eu-central-1.getblock.io/<ACCESS-TOKEN>/', headers={'Content-Type': 'application/json'}, json={'jsonrpc': '2.0', 'id': 'getblock.io', 'method': 'check_tx', 'params': {"tx": "Cr0BC..."}})
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
    let res = client.post("https://shared.eu-central-1.getblock.io/<ACCESS-TOKEN>/").json(&json!({"jsonrpc":"2.0","id":"getblock.io","method":"check_tx","params":{"tx": "Cr0BC..."}})).send().await?.json::<Value>().await?;
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
        "code": 0,
        "gas_wanted": "200000",
        "gas_used": "118000",
        "log": "[]"
    }
}
```

## Response Fields

| Field     | Type    | Description               |
| --------- | ------- | ------------------------- |
| code      | integer | CheckTx code; 0 = valid   |
| gas\_used | string  | Gas used during the check |
| log       | string  | Log on failure            |

## Use Cases

* **Pre-validation**: Test a transaction before broadcasting
* **Fee Sizing**: Read gas\_used to size fees

## Error Handling

| Error                     | Message        | Description                                       |
| ------------------------- | -------------- | ------------------------------------------------- |
| -32603 / Internal error   | Internal error | Undecodable transaction                           |
| 403 / RBAC: access denied | Access denied  | The GetBlock access token is missing or incorrect |
