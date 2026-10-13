---
description: >-
  Example code for the iota_isTransactionIndexedOnNode JSON-RPC method. Complete
  guide on how to use iota_isTransactionIndexedOnNode JSON-RPC method in
  GetBlock Web3 documentation.
---

# iota\_isTransactionIndexedOnNode - IOTA

Returns whether a given transaction digest has been indexed on the serving node, so a client knows when read-after-write queries will succeed.

## Parameters

| Parameter | Type   | Required | Description              |
| --------- | ------ | -------- | ------------------------ |
| digest    | string | Yes      | Transaction block digest |

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
    "method": "iota_isTransactionIndexedOnNode",
    "params": ["AdE..."]
}'
```
{% endcode %}
{% endtab %}

{% tab title="Axios" %}
{% code title="example.js" %}
```javascript
const axios = require('axios');
const response = await axios.post('https://shared.eu-central-1.getblock.io/<ACCESS-TOKEN>/', { jsonrpc: '2.0', id: 'getblock.io', method: 'iota_isTransactionIndexedOnNode', params: ["AdE..."] }, { headers: { 'Content-Type': 'application/json' } });
console.log(response.data.result);
```
{% endcode %}
{% endtab %}

{% tab title="Request" %}
{% code title="example.py" %}
```python
import requests
response = requests.post('https://shared.eu-central-1.getblock.io/<ACCESS-TOKEN>/', headers={'Content-Type': 'application/json'}, json={'jsonrpc': '2.0', 'id': 'getblock.io', 'method': 'iota_isTransactionIndexedOnNode', 'params': ["AdE..."]})
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
    let res = client.post("https://shared.eu-central-1.getblock.io/<ACCESS-TOKEN>/").json(&json!({"jsonrpc":"2.0","id":"getblock.io","method":"iota_isTransactionIndexedOnNode","params":["AdE..."]})).send().await?.json::<Value>().await?;
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
    "result": true
}
```

## Response Parameters

| Field   | Type    | Description                                     |
| ------- | ------- | ----------------------------------------------- |
| jsonrpc | string  | JSON-RPC version ("2.0")                        |
| id      | string  | Request identifier ("getblock.io")              |
| result  | boolean | True if the transaction is indexed on this node |

## Use Cases

* **Read-After-Write**: Poll until a submitted transaction is queryable

## Error Handling

| Error                     | Message          | Description                                       |
| ------------------------- | ---------------- | ------------------------------------------------- |
| -32602                    | Invalid params   | A parameter was missing or malformed              |
| -32601                    | Method not found | The method name is misspelled or not served       |
| 403 / RBAC: access denied | Access denied    | The GetBlock access token is missing or incorrect |
