---
description: >-
  Example code for the zks_getProtocolVersion JSON-RPC method. Complete guide on
  how to use zks_getProtocolVersion JSON-RPC in GetBlock Web3 documentation.
---

# zks\_getProtocolVersion - Abstract

Returns the current ZK Stack protocol version, including its id, timestamp, and the base system contract hashes in effect.

## Parameters

{% hint style="info" %}
This method takes an empty `params` array.
{% endhint %}

## Request

{% tabs %}
{% tab title="cURL" %}
{% code overflow="wrap" %}
```bash
curl --location --request POST 'https://shared.eu-central-1.getblock.io/<ACCESS-TOKEN>/' \
--header 'Content-Type: application/json' \
--data-raw '{
    "jsonrpc": "2.0",
    "method": "zks_getProtocolVersion",
    "params": [],
    "id": "getblock.io"
}'
```
{% endcode %}
{% endtab %}

{% tab title="Axios" %}
{% code title="example.js" %}
```javascript
const axios = require('axios');
const response = await axios.post('https://shared.eu-central-1.getblock.io/<ACCESS-TOKEN>/', { jsonrpc: '2.0', method: 'zks_getProtocolVersion', params: [], id: 'getblock.io' }, { headers: { 'Content-Type': 'application/json' } });
console.log(response.data.result);
```
{% endcode %}
{% endtab %}

{% tab title="Request" %}
{% code title="example.py" %}
```python
import requests
response = requests.post('https://shared.eu-central-1.getblock.io/<ACCESS-TOKEN>/', headers={'Content-Type': 'application/json'}, json={'jsonrpc': '2.0', 'method': 'zks_getProtocolVersion', 'params': [], 'id': 'getblock.io'})
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
    let res = client.post("https://shared.eu-central-1.getblock.io/<ACCESS-TOKEN>/").json(&json!({"jsonrpc":"2.0","method":"zks_getProtocolVersion","params":[],"id":"getblock.io"})).send().await?.json::<Value>().await?;
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
        "version_id": 24,
        "timestamp": 1730000000,
        "base_system_contracts": {
            "bootloader": "0x...",
            "default_aa": "0x..."
        }
    }
}
```

## Response Fields

| Field                   | Type    | Description                                                |
| ----------------------- | ------- | ---------------------------------------------------------- |
| version\_id             | integer | Protocol version id                                        |
| base\_system\_contracts | object  | Bootloader and default account-abstraction contract hashes |

## Use Cases

* **Compatibility**: Adapt to the active protocol version
* **Upgrades**: Detect protocol upgrades

## Error Handling

| Error                     | Message        | Description                                       |
| ------------------------- | -------------- | ------------------------------------------------- |
| -32602 / Invalid params   | Invalid params | A parameter was malformed                         |
| 403 / RBAC: access denied | Access denied  | The GetBlock access token is missing or incorrect |
