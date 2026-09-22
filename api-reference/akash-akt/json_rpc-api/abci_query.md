---
description: >-
  Example code for the abci_query JSON-RPC method. Complete guide on how to
  use abci_query JSON-RPC in GetBlock Web3 documentation.
---

# abci\_query - Akash

Executes an ABCI query against application state at a path, returning the raw (base64) value. The low-level way to read any Cosmos SDK or Akash module store; underlies the REST gateway.

## Parameters

| Parameter | Type    | Required | Description             |
| --------- | ------- | -------- | ----------------------- |
| path      | string  | Yes      | ABCI query path         |
| data      | string  | Yes      | Hex query request bytes |
| height    | string  | Optional | Height (0=latest)       |
| prove     | boolean | Optional | Include proof           |

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
    "method": "abci_query",
    "params": {"path": "/cosmos.bank.v1beta1.Query/AllBalances", "data": "0a2b...", "height": "0", "prove": false}
}'
```
{% endcode %}
{% endtab %}

{% tab title="Axios" %}
{% code title="example.js" %}
```javascript
const axios = require('axios');
const response = await axios.post('https://shared.eu-central-1.getblock.io/<ACCESS-TOKEN>/', { jsonrpc: '2.0', id: 'getblock.io', method: 'abci_query', params: {"path": "/cosmos.bank.v1beta1.Query/AllBalances", "data": "0a2b...", "height": "0", "prove": false} }, { headers: { 'Content-Type': 'application/json' } });
console.log(response.data.result);
```
{% endcode %}
{% endtab %}

{% tab title="Request" %}
{% code title="example.py" %}
```python
import requests
response = requests.post('https://shared.eu-central-1.getblock.io/<ACCESS-TOKEN>/', headers={'Content-Type': 'application/json'}, json={'jsonrpc': '2.0', 'id': 'getblock.io', 'method': 'abci_query', 'params': {"path": "/cosmos.bank.v1beta1.Query/AllBalances", "data": "0a2b...", "height": "0", "prove": false}})
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
    let res = client.post("https://shared.eu-central-1.getblock.io/<ACCESS-TOKEN>/").json(&json!({"jsonrpc":"2.0","id":"getblock.io","method":"abci_query","params":{"path": "/cosmos.bank.v1beta1.Query/AllBalances", "data": "0a2b...", "height": "0", "prove": false}})).send().await?.json::<Value>().await?;
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
        "response": {
            "code": 0,
            "value": "Cg8KA3Vha3QSBDEwMDA=",
            "height": "19500000"
        }
    }
}
```

## Response Fields

| Field           | Type    | Description              |
| --------------- | ------- | ------------------------ |
| response.value  | string  | Base64 protobuf response |
| response.code   | integer | ABCI code; 0 on success  |
| response.height | string  | Height answered at       |

## Use Cases

* **Generic Reads**: Query any module store
* **Akash Modules**: Read deployment/market/provider state
* **Light Clients**: Read verified state

## Error Handling

| Error                     | Message        | Description                                       |
| ------------------------- | -------------- | ------------------------------------------------- |
| -32603 / Internal error   | Internal error | Unknown path or bad data                          |
| 403 / RBAC: access denied | Access denied  | The GetBlock access token is missing or incorrect |
