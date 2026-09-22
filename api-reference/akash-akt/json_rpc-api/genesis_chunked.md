# genesis\_chunked

Returns a base64 chunk of the genesis document, for genesis files too large to return at once.

## Parameters

| Parameter | Type   | Required | Description            |
| --------- | ------ | -------- | ---------------------- |
| chunk     | string | Yes      | Zero-based chunk index |

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
    "method": "genesis_chunked",
    "params": {"chunk": "0"}
}'
```
{% endcode %}
{% endtab %}

{% tab title="Axios" %}
{% code title="example.js" %}
```javascript
const axios = require('axios');
const response = await axios.post('https://shared.eu-central-1.getblock.io/<ACCESS-TOKEN>/', { jsonrpc: '2.0', id: 'getblock.io', method: 'genesis_chunked', params: {"chunk": "0"} }, { headers: { 'Content-Type': 'application/json' } });
console.log(response.data.result);
```
{% endcode %}
{% endtab %}

{% tab title="Request" %}
{% code title="example.py" %}
```python
import requests
response = requests.post('https://shared.eu-central-1.getblock.io/<ACCESS-TOKEN>/', headers={'Content-Type': 'application/json'}, json={'jsonrpc': '2.0', 'id': 'getblock.io', 'method': 'genesis_chunked', 'params': {"chunk": "0"}})
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
    let res = client.post("https://shared.eu-central-1.getblock.io/<ACCESS-TOKEN>/").json(&json!({"jsonrpc":"2.0","id":"getblock.io","method":"genesis_chunked","params":{"chunk": "0"}})).send().await?.json::<Value>().await?;
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
        "chunk": 0,
        "total": 10,
        "data": "eyJ..."
    }
}
```

## Response Fields

| Field | Type    | Description            |
| ----- | ------- | ---------------------- |
| data  | string  | Base64 genesis chunk   |
| total | integer | Total number of chunks |

## Use Cases

* **Large Genesis**: Fetch genesis in pieces
* **Bootstrapping**: Reassemble the genesis file

## Error Handling

| Error                     | Message        | Description                                       |
| ------------------------- | -------------- | ------------------------------------------------- |
| -32603 / Internal error   | Internal error | Invalid chunk index                               |
| 403 / RBAC: access denied | Access denied  | The GetBlock access token is missing or incorrect |
