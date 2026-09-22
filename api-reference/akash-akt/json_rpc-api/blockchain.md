# blockchain

Returns block headers for a range of heights (max 20 per call), newest first.

## Parameters

| Parameter | Type   | Required | Description        |
| --------- | ------ | -------- | ------------------ |
| minHeight | string | Optional | Lower height bound |
| maxHeight | string | Optional | Upper height bound |

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
    "method": "blockchain",
    "params": {"minHeight": "19499990", "maxHeight": "19500000"}
}'
```
{% endcode %}
{% endtab %}

{% tab title="Axios" %}
{% code title="example.js" %}
```javascript
const axios = require('axios');
const response = await axios.post('https://shared.eu-central-1.getblock.io/<ACCESS-TOKEN>/', { jsonrpc: '2.0', id: 'getblock.io', method: 'blockchain', params: {"minHeight": "19499990", "maxHeight": "19500000"} }, { headers: { 'Content-Type': 'application/json' } });
console.log(response.data.result);
```
{% endcode %}
{% endtab %}

{% tab title="Request" %}
{% code title="example.py" %}
```python
import requests
response = requests.post('https://shared.eu-central-1.getblock.io/<ACCESS-TOKEN>/', headers={'Content-Type': 'application/json'}, json={'jsonrpc': '2.0', 'id': 'getblock.io', 'method': 'blockchain', 'params': {"minHeight": "19499990", "maxHeight": "19500000"}})
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
    let res = client.post("https://shared.eu-central-1.getblock.io/<ACCESS-TOKEN>/").json(&json!({"jsonrpc":"2.0","id":"getblock.io","method":"blockchain","params":{"minHeight": "19499990", "maxHeight": "19500000"}})).send().await?.json::<Value>().await?;
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
        "last_height": "19500000",
        "block_metas": [
            {
                "block_id": {
                    "hash": "E1F2..."
                },
                "header": {
                    "height": "19500000",
                    "time": "2025-11-01T12:00:00Z"
                }
            }
        ]
    }
}
```

## Response Fields

| Field        | Type   | Description                            |
| ------------ | ------ | -------------------------------------- |
| block\_metas | array  | Block metadata (id, header) per height |
| last\_height | string | Current chain height                   |

## Use Cases

* **Header Sync**: Fetch headers in a range
* **Explorers**: List recent blocks

## Error Handling

| Error                     | Message        | Description                                       |
| ------------------------- | -------------- | ------------------------------------------------- |
| -32603 / Internal error   | Internal error | Range too large or out of bounds                  |
| 403 / RBAC: access denied | Access denied  | The GetBlock access token is missing or incorrect |
