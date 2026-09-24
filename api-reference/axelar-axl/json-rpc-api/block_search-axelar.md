---
description: >-
  Example code for the block_search JSON-RPC method. Complete guide on how to
  use block_search JSON-RPC in GetBlock Web3 documentation.
---

# block\_search - Axelar

Returns blocks matching a block-event query (for example by begin/end-block events), paginated. Requires block indexing.

## Parameters

| Parameter | Type   | Required | Description       |
| --------- | ------ | -------- | ----------------- |
| query     | string | Yes      | Block-event query |
| page      | string | Optional | Page              |
| per\_page | string | Optional | Results per page  |
| order\_by | string | Optional | asc or desc       |

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
    "method": "block_search",
    "params": {"query": "block.height > 19000000", "page": "1", "per_page": "20", "order_by": "desc"}
}'
```
{% endcode %}
{% endtab %}

{% tab title="Axios" %}
{% code title="example.js" %}
```javascript
const axios = require('axios');
const response = await axios.post('https://shared.eu-central-1.getblock.io/<ACCESS-TOKEN>/', { jsonrpc: '2.0', id: 'getblock.io', method: 'block_search', params: {"query": "block.height > 19000000", "page": "1", "per_page": "20", "order_by": "desc"} }, { headers: { 'Content-Type': 'application/json' } });
console.log(response.data.result);
```
{% endcode %}
{% endtab %}

{% tab title="Request" %}
{% code title="example.py" %}
```python
import requests
response = requests.post('https://shared.eu-central-1.getblock.io/<ACCESS-TOKEN>/', headers={'Content-Type': 'application/json'}, json={'jsonrpc': '2.0', 'id': 'getblock.io', 'method': 'block_search', 'params': {"query": "block.height > 19000000", "page": "1", "per_page": "20", "order_by": "desc"}})
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
    let res = client.post("https://shared.eu-central-1.getblock.io/<ACCESS-TOKEN>/").json(&json!({"jsonrpc":"2.0","id":"getblock.io","method":"block_search","params":{"query": "block.height > 19000000", "page": "1", "per_page": "20", "order_by": "desc"}})).send().await?.json::<Value>().await?;
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
        "blocks": [
            {
                "block_id": {
                    "hash": "E1F2..."
                },
                "block": {
                    "header": {
                        "height": "19500000"
                    }
                }
            }
        ],
        "total_count": "1"
    }
}
```

## Response Fields

| Field        | Type   | Description     |
| ------------ | ------ | --------------- |
| blocks       | array  | Matching blocks |
| total\_count | string | Total matches   |

## Use Cases

* **Event Search**: Find blocks by begin/end-block events
* **Indexing**: Locate blocks by criteria

## Error Handling

| Error                     | Message        | Description                                       |
| ------------------------- | -------------- | ------------------------------------------------- |
| -32603 / Internal error   | Internal error | Block indexing disabled or bad query              |
| 403 / RBAC: access denied | Access denied  | The GetBlock access token is missing or incorrect |
