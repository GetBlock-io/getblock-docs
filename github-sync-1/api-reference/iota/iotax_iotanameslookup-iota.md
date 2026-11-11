---
description: >-
  Example code for the iotax_iotaNamesLookup JSON-RPC method. Complete guide on
  how to use iotax_iotaNamesLookup JSON-RPC method in GetBlock Web3
  documentation.
---

# iotax\_iotaNamesLookup - IOTA

Resolves an IOTA name (the IOTA naming service) to its registry record, including the address it points to.

## Parameters

| Parameter | Type   | Required | Description                  |
| --------- | ------ | -------- | ---------------------------- |
| name      | string | Yes      | IOTA name, e.g. example.iota |

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
    "method": "iotax_iotaNamesLookup",
    "params": ["example.iota"]
}'
```
{% endcode %}
{% endtab %}

{% tab title="Axios" %}
{% code title="example.js" %}
```javascript
const axios = require('axios');
const response = await axios.post('https://shared.eu-central-1.getblock.io/<ACCESS-TOKEN>/', { jsonrpc: '2.0', id: 'getblock.io', method: 'iotax_iotaNamesLookup', params: ["example.iota"] }, { headers: { 'Content-Type': 'application/json' } });
console.log(response.data.result);
```
{% endcode %}
{% endtab %}

{% tab title="Request" %}
{% code title="example.py" %}
```python
import requests
response = requests.post('https://shared.eu-central-1.getblock.io/<ACCESS-TOKEN>/', headers={'Content-Type': 'application/json'}, json={'jsonrpc': '2.0', 'id': 'getblock.io', 'method': 'iotax_iotaNamesLookup', 'params': ["example.iota"]})
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
    let res = client.post("https://shared.eu-central-1.getblock.io/<ACCESS-TOKEN>/").json(&json!({"jsonrpc":"2.0","id":"getblock.io","method":"iotax_iotaNamesLookup","params":["example.iota"]})).send().await?.json::<Value>().await?;
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
        "id": "0x...",
        "targetAddress": "0xabc...",
        "expirationTimestampMs": "1768308000000",
        "data": {}
    }
}
```

## Response Parameters

| Field                        | Type   | Description                        |
| ---------------------------- | ------ | ---------------------------------- |
| jsonrpc                      | string | JSON-RPC version ("2.0")           |
| id                           | string | Request identifier ("getblock.io") |
| result.targetAddress         | string | Address the name resolves to       |
| result.expirationTimestampMs | string | Registration expiry                |

## Use Cases

* **Name Resolution**: Resolve a human-readable name to an address
* **UX**: Display names instead of raw addresses

## Error Handling

| Error                     | Message          | Description                                       |
| ------------------------- | ---------------- | ------------------------------------------------- |
| -32602                    | Invalid params   | A parameter was missing or malformed              |
| -32601                    | Method not found | The method name is misspelled or not served       |
| 403 / RBAC: access denied | Access denied    | The GetBlock access token is missing or incorrect |
