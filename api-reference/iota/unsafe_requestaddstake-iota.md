---
description: >-
  Example code for the unsafe_requestAddStake JSON-RPC method. Complete guide on
  how to use unsafe_requestAddStake JSON-RPC method in GetBlock Web3
  documentation.
---

# unsafe\_requestAddStake - IOTA

Builds an unsigned transaction that stakes IOTA coins with a validator, creating a delegated stake.

## Parameters

| Parameter   | Type   | Required | Description                   |
| ----------- | ------ | -------- | ----------------------------- |
| signer      | string | Yes      | Staker address                |
| coins       | array  | Yes      | IOTA coin object IDs to stake |
| amount      | string | Optional | Amount to stake               |
| validator   | string | Yes      | Validator address             |
| gas         | string | Optional | Gas coin object ID            |
| gas\_budget | string | Yes      | Gas budget                    |

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
    "method": "unsafe_requestAddStake",
    "params": ["0xabc...", ["0x9f..."], "50000000000", "0xval...", null, "10000000"]
}'
```
{% endcode %}
{% endtab %}

{% tab title="Axios" %}
{% code title="example.js" %}
```javascript
const axios = require('axios');
const response = await axios.post('https://shared.eu-central-1.getblock.io/<ACCESS-TOKEN>/', { jsonrpc: '2.0', id: 'getblock.io', method: 'unsafe_requestAddStake', params: ["0xabc...", ["0x9f..."], "50000000000", "0xval...", null, "10000000"] }, { headers: { 'Content-Type': 'application/json' } });
console.log(response.data.result);
```
{% endcode %}
{% endtab %}

{% tab title="Request" %}
{% code title="example.py" %}
```python
import requests
response = requests.post('https://shared.eu-central-1.getblock.io/<ACCESS-TOKEN>/', headers={'Content-Type': 'application/json'}, json={'jsonrpc': '2.0', 'id': 'getblock.io', 'method': 'unsafe_requestAddStake', 'params': ["0xabc...", ["0x9f..."], "50000000000", "0xval...", null, "10000000"]})
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
    let res = client.post("https://shared.eu-central-1.getblock.io/<ACCESS-TOKEN>/").json(&json!({"jsonrpc":"2.0","id":"getblock.io","method":"unsafe_requestAddStake","params":["0xabc...", ["0x9f..."], "50000000000", "0xval...", null, "10000000"]})).send().await?.json::<Value>().await?;
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

* **Staking**: Build a delegation transaction

## Error Handling

| Error                     | Message          | Description                                       |
| ------------------------- | ---------------- | ------------------------------------------------- |
| -32602                    | Invalid params   | A parameter was missing or malformed              |
| -32601                    | Method not found | The method name is misspelled or not served       |
| 403 / RBAC: access denied | Access denied    | The GetBlock access token is missing or incorrect |
