---
description: >-
  Example code for the iota_getEvents JSON-RPC method. Complete guide on how to
  use iota_getEvents JSON-RPC method in GetBlock Web3 documentation.
---

# iota\_getEvents - IOTA

Returns all events emitted by a given transaction block.

## Parameters

| Parameter           | Type   | Required | Description              |
| ------------------- | ------ | -------- | ------------------------ |
| transaction\_digest | string | Yes      | Transaction block digest |

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
    "method": "iota_getEvents",
    "params": ["AdE..."]
}'
```
{% endcode %}
{% endtab %}

{% tab title="Axios" %}
{% code title="example.js" %}
```javascript
const axios = require('axios');
const response = await axios.post('https://shared.eu-central-1.getblock.io/<ACCESS-TOKEN>/', { jsonrpc: '2.0', id: 'getblock.io', method: 'iota_getEvents', params: ["AdE..."] }, { headers: { 'Content-Type': 'application/json' } });
console.log(response.data.result);
```
{% endcode %}
{% endtab %}

{% tab title="Request" %}
{% code title="example.py" %}
```python
import requests
response = requests.post('https://shared.eu-central-1.getblock.io/<ACCESS-TOKEN>/', headers={'Content-Type': 'application/json'}, json={'jsonrpc': '2.0', 'id': 'getblock.io', 'method': 'iota_getEvents', 'params': ["AdE..."]})
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
    let res = client.post("https://shared.eu-central-1.getblock.io/<ACCESS-TOKEN>/").json(&json!({"jsonrpc":"2.0","id":"getblock.io","method":"iota_getEvents","params":["AdE..."]})).send().await?.json::<Value>().await?;
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
        "data": [
            {
                "id": {
                    "txDigest": "AdE...",
                    "eventSeq": "0"
                },
                "packageId": "0x3...",
                "transactionModule": "coin",
                "type": "0x2::balance::BalanceEvent",
                "parsedJson": {
                    "amount": "1000000000"
                }
            }
        ]
    }
}
```

## Response Parameters

| Field       | Type   | Description                                  |
| ----------- | ------ | -------------------------------------------- |
| jsonrpc     | string | JSON-RPC version ("2.0")                     |
| id          | string | Request identifier ("getblock.io")           |
| result.data | array  | Events with id, type, and parsedJson payload |

## Use Cases

* **Event Processing**: Read events emitted by a transaction
* **Indexing**: Track contract activity

## Error Handling

| Error                     | Message          | Description                                       |
| ------------------------- | ---------------- | ------------------------------------------------- |
| -32602                    | Invalid params   | A parameter was missing or malformed              |
| -32601                    | Method not found | The method name is misspelled or not served       |
| 403 / RBAC: access denied | Access denied    | The GetBlock access token is missing or incorrect |
