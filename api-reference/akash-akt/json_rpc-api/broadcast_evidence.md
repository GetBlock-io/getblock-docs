---
description: >-
  Example code for the broadcast_evidence JSON-RPC method. Complete guide on
  how to use broadcast_evidence JSON-RPC in GetBlock Web3 documentation.
---

# broadcast\_evidence - Akash

Submits evidence of validator misbehaviour (for example double-signing) to the node, returning the evidence hash.

## Parameters

| Parameter | Type   | Required | Description           |
| --------- | ------ | -------- | --------------------- |
| evidence  | string | Yes      | JSON-encoded evidence |

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
    "method": "broadcast_evidence",
    "params": {"evidence": "{...}"}
}'
```
{% endcode %}
{% endtab %}

{% tab title="Axios" %}
{% code title="example.js" %}
```javascript
const axios = require('axios');
const response = await axios.post('https://shared.eu-central-1.getblock.io/<ACCESS-TOKEN>/', { jsonrpc: '2.0', id: 'getblock.io', method: 'broadcast_evidence', params: {"evidence": "{...}"} }, { headers: { 'Content-Type': 'application/json' } });
console.log(response.data.result);
```
{% endcode %}
{% endtab %}

{% tab title="Request" %}
{% code title="example.py" %}
```python
import requests
response = requests.post('https://shared.eu-central-1.getblock.io/<ACCESS-TOKEN>/', headers={'Content-Type': 'application/json'}, json={'jsonrpc': '2.0', 'id': 'getblock.io', 'method': 'broadcast_evidence', 'params': {"evidence": "{...}"}})
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
    let res = client.post("https://shared.eu-central-1.getblock.io/<ACCESS-TOKEN>/").json(&json!({"jsonrpc":"2.0","id":"getblock.io","method":"broadcast_evidence","params":{"evidence": "{...}"}})).send().await?.json::<Value>().await?;
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
    "result": "3A1F9C2E7B4D8A05F6C1E3D9B2A4C6E8F0D1B3A5C7E9F2D4B6A8C0E1F3D5B7A9C"
}
```

## Response Fields

| Field  | Type   | Description                    |
| ------ | ------ | ------------------------------ |
| result | string | Hash of the submitted evidence |

## Use Cases

* **Slashing**: Report validator misbehaviour
* **Security**: Submit double-sign evidence

## Error Handling

| Error                     | Message        | Description                                       |
| ------------------------- | -------------- | ------------------------------------------------- |
| -32603 / Internal error   | Internal error | Invalid evidence                                  |
| 403 / RBAC: access denied | Access denied  | The GetBlock access token is missing or incorrect |
