---
description: >-
  Example code for the chainHead_v1_header JSON-RPC method. Complete guide on
  how to use chainHead_v1_header JSON-RPC in GetBlock Web3 documentation.
---

# chainHead\_v1\_header - Peaq

Retrieves the header of a followed block via the chainHead API.

## Parameters

| # | Example              | Description            |
| - | -------------------- | ---------------------- |
| 0 | `followSubscription` | Positional parameter 0 |
| 1 | `0xblockhash`        | Positional parameter 1 |

## Request

{% tabs %}
{% tab title="cURL" %}
{% code overflow="wrap" %}
```bash
curl --location --request POST 'https://shared.us-east-1.getblock.io/<ACCESS-TOKEN>/' \
--header 'Content-Type: application/json' \
--data-raw '{
    "jsonrpc": "2.0",
    "id": "getblock.io",
    "method": "chainHead_v1_header",
    "params": ["followSubscription", "0xblockhash"]
}'
```
{% endcode %}
{% endtab %}

{% tab title="Axios" %}
{% code title="example.js" %}
```javascript
const axios = require('axios');
const response = await axios.post('https://shared.us-east-1.getblock.io/<ACCESS-TOKEN>/', { jsonrpc: '2.0', id: 'getblock.io', method: 'chainHead_v1_header', params: ["followSubscription", "0xblockhash"] }, { headers: { 'Content-Type': 'application/json' } });
console.log(response.data.result);
```
{% endcode %}
{% endtab %}

{% tab title="Request" %}
{% code title="example.py" %}
```python
import requests
response = requests.post('https://shared.us-east-1.getblock.io/<ACCESS-TOKEN>/', headers={'Content-Type': 'application/json'}, json={'jsonrpc': '2.0', 'id': 'getblock.io', 'method': 'chainHead_v1_header', 'params': ["followSubscription", "0xblockhash"]})
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
    let res = client.post("https://shared.us-east-1.getblock.io/<ACCESS-TOKEN>/").json(&json!({"jsonrpc":"2.0","id":"getblock.io","method":"chainHead_v1_header","params":["followSubscription", "0xblockhash"]})).send().await?.json::<Value>().await?;
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
    "result": "0xscaleheader"
}
```

## Response Parameters

| Field   | Type   | Description                           |
| ------- | ------ | ------------------------------------- |
| jsonrpc | string | JSON-RPC version ("2.0")              |
| id      | string | Request identifier ("getblock.io")    |
| result  | varies | The method result (see example above) |

## Use Cases

* Programmatic access to the `chainHead_v1_header` method on peaq
* Integrations, indexing, and tooling built on the peaq node API

## Error Handling

| Error                     | Message          | Description                                       |
| ------------------------- | ---------------- | ------------------------------------------------- |
| -32602                    | Invalid params   | A parameter was missing or malformed              |
| -32601                    | Method not found | The method name is not exposed by the node        |
| 403 / RBAC: access denied | Access denied    | The GetBlock access token is missing or incorrect |
