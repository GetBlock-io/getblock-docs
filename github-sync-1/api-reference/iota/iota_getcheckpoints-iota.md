---
description: >-
  Example code for the iota_getCheckpoints JSON-RPC method. Complete guide on
  how to use iota_getCheckpoints JSON-RPC method in GetBlock Web3 documentation.
---

# iota\_getCheckpoints - IOTA

Returns a paginated list of checkpoints in ascending or descending order from a cursor.

## Parameters

| Parameter         | Type    | Required | Description                                |
| ----------------- | ------- | -------- | ------------------------------------------ |
| cursor            | string  | Optional | Paging cursor (checkpoint sequence number) |
| limit             | integer | Optional | Max checkpoints to return                  |
| descending\_order | boolean | Optional | Return newest first when true              |

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
    "method": "iota_getCheckpoints",
    "params": [null, 5, true]
}'
```
{% endcode %}
{% endtab %}

{% tab title="Axios" %}
{% code title="example.js" %}
```javascript
const axios = require('axios');
const response = await axios.post('https://shared.eu-central-1.getblock.io/<ACCESS-TOKEN>/', { jsonrpc: '2.0', id: 'getblock.io', method: 'iota_getCheckpoints', params: [null, 5, true] }, { headers: { 'Content-Type': 'application/json' } });
console.log(response.data.result);
```
{% endcode %}
{% endtab %}

{% tab title="Request" %}
{% code title="example.py" %}
```python
import requests
response = requests.post('https://shared.eu-central-1.getblock.io/<ACCESS-TOKEN>/', headers={'Content-Type': 'application/json'}, json={'jsonrpc': '2.0', 'id': 'getblock.io', 'method': 'iota_getCheckpoints', 'params': [null, 5, true]})
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
    let res = client.post("https://shared.eu-central-1.getblock.io/<ACCESS-TOKEN>/").json(&json!({"jsonrpc":"2.0","id":"getblock.io","method":"iota_getCheckpoints","params":[null, 5, true]})).send().await?.json::<Value>().await?;
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
                "sequenceNumber": "128344567",
                "digest": "7Md...",
                "timestampMs": "1736772000000"
            }
        ],
        "nextCursor": "128344562",
        "hasNextPage": true
    }
}
```

## Response Parameters

| Field              | Type    | Description                        |
| ------------------ | ------- | ---------------------------------- |
| jsonrpc            | string  | JSON-RPC version ("2.0")           |
| id                 | string  | Request identifier ("getblock.io") |
| result.data        | array   | Checkpoint summaries               |
| result.nextCursor  | string  | Cursor for the next page           |
| result.hasNextPage | boolean | Whether more pages exist           |

## Use Cases

* **History Paging**: Walk checkpoints in bulk
* **Backfill**: Reindex a range of the chain

## Error Handling

| Error                     | Message          | Description                                       |
| ------------------------- | ---------------- | ------------------------------------------------- |
| -32602                    | Invalid params   | A parameter was missing or malformed              |
| -32601                    | Method not found | The method name is misspelled or not served       |
| 403 / RBAC: access denied | Access denied    | The GetBlock access token is missing or incorrect |
