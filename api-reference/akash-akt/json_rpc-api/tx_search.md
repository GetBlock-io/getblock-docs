# tx\_search

Returns transactions matching an event query (by sender, recipient, height, etc.), paginated. Requires transaction indexing.

## Parameters

| Parameter | Type   | Required | Description      |
| --------- | ------ | -------- | ---------------- |
| query     | string | Yes      | Event query      |
| page      | string | Optional | Page             |
| per\_page | string | Optional | Results per page |
| order\_by | string | Optional | asc or desc      |

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
    "method": "tx_search",
    "params": {"query": "message.sender='akash1nl7dg3xj9j2y6q7z0v4w8c2b5d1f3g6h9k2m4p'", "page": "1", "per_page": "30", "order_by": "desc"}
}'
```
{% endcode %}
{% endtab %}

{% tab title="Axios" %}
{% code title="example.js" %}
```javascript
const axios = require('axios');
const response = await axios.post('https://shared.eu-central-1.getblock.io/<ACCESS-TOKEN>/', { jsonrpc: '2.0', id: 'getblock.io', method: 'tx_search', params: {"query": "message.sender='akash1nl7dg3xj9j2y6q7z0v4w8c2b5d1f3g6h9k2m4p'", "page": "1", "per_page": "30", "order_by": "desc"} }, { headers: { 'Content-Type': 'application/json' } });
console.log(response.data.result);
```
{% endcode %}
{% endtab %}

{% tab title="Request" %}
{% code title="example.py" %}
```python
import requests
response = requests.post('https://shared.eu-central-1.getblock.io/<ACCESS-TOKEN>/', headers={'Content-Type': 'application/json'}, json={'jsonrpc': '2.0', 'id': 'getblock.io', 'method': 'tx_search', 'params': {"query": "message.sender='akash1nl7dg3xj9j2y6q7z0v4w8c2b5d1f3g6h9k2m4p'", "page": "1", "per_page": "30", "order_by": "desc"}})
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
    let res = client.post("https://shared.eu-central-1.getblock.io/<ACCESS-TOKEN>/").json(&json!({"jsonrpc":"2.0","id":"getblock.io","method":"tx_search","params":{"query": "message.sender='akash1nl7dg3xj9j2y6q7z0v4w8c2b5d1f3g6h9k2m4p'", "page": "1", "per_page": "30", "order_by": "desc"}})).send().await?.json::<Value>().await?;
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
    "txs": [
      {
        "hash": "3A1F9C2E7B4D8A05F6C1E3D9B2A4C6E8F0D1B3A5C7E9F2D4B6A8C0E1F3D5B7A9C",
        "height": "19500000",
        "tx_result": {
          "code": 0
        }
      }
    ],
    "total_count": "1"
  }
}
```

## Response Fields

| Field        | Type   | Description           |
| ------------ | ------ | --------------------- |
| txs          | array  | Matching transactions |
| total\_count | string | Total matches         |

## Use Cases

* **Account History**: List txs for an address
* **Indexing**: Backfill by event filter
* **Analytics**: Query by message type

## Error Handling

| Error                     | Message        | Description                                       |
| ------------------------- | -------------- | ------------------------------------------------- |
| -32603 / Internal error   | Internal error | Indexing disabled or bad query                    |
| 403 / RBAC: access denied | Access denied  | The GetBlock access token is missing or incorrect |
