---
description: >-
  Example code for the block_by_hash JSON-RPC method. Complete guide on how to
  use block_by_hash JSON-RPC in GetBlock Web3 documentation.
---

# block\_by\_hash - Akash

Returns the block identified by its hash.

## Parameters

| Parameter | Type   | Required | Description              |
| --------- | ------ | -------- | ------------------------ |
| hash      | string | Yes      | Block hash (0x-prefixed) |

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
    "method": "block_by_hash",
    "params": {"hash": "0xE1F2..."}
}'
```
{% endcode %}
{% endtab %}

{% tab title="Axios" %}
{% code title="example.js" %}
```javascript
const axios = require('axios');
const response = await axios.post('https://shared.eu-central-1.getblock.io/<ACCESS-TOKEN>/', { jsonrpc: '2.0', id: 'getblock.io', method: 'block_by_hash', params: {"hash": "0xE1F2..."} }, { headers: { 'Content-Type': 'application/json' } });
console.log(response.data.result);
```
{% endcode %}
{% endtab %}

{% tab title="Request" %}
{% code title="example.py" %}
```python
import requests
response = requests.post('https://shared.eu-central-1.getblock.io/<ACCESS-TOKEN>/', headers={'Content-Type': 'application/json'}, json={'jsonrpc': '2.0', 'id': 'getblock.io', 'method': 'block_by_hash', 'params': {"hash": "0xE1F2..."}})
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
    let res = client.post("https://shared.eu-central-1.getblock.io/<ACCESS-TOKEN>/").json(&json!({"jsonrpc":"2.0","id":"getblock.io","method":"block_by_hash","params":{"hash": "0xE1F2..."}})).send().await?.json::<Value>().await?;
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
        "block_id": {
            "hash": "E1F2..."
        },
        "block": {
            "header": {
                "height": "19500000"
            }
        }
    }
}
```

## Response Fields

| Field     | Type   | Description           |
| --------- | ------ | --------------------- |
| block     | object | Block header and data |
| block\_id | object | Block hash            |

## Use Cases

* **Block Lookup**: Fetch a block by hash
* **Explorers**: Resolve a hash to a block

## Error Handling

| Error                     | Message       | Description                                       |
| ------------------------- | ------------- | ------------------------------------------------- |
| -32603 / Internal error   | Not found     | No block with that hash                           |
| 403 / RBAC: access denied | Access denied | The GetBlock access token is missing or incorrect |
