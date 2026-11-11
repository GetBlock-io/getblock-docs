---
description: >-
  Example code for the /v1/scripts REST method. Complete guide on how to use the
  /v1/scripts REST method in GetBlock Web3 documentation
---

# execute-script - Flow

Executes a read-only Cadence script against the chain state and returns the base64-encoded JSON-Cadence result. Scripts cannot change state. Pin execution to a block with the `block_height` or `block_id` query parameter.

## Endpoint

```http
POST /v1/scripts
```

## Query Parameters

| Parameter     | Type   | Description                     |
| ------------- | ------ | ------------------------------- |
| block\_height | string | Block height, or final / sealed |
| block\_id     | string | Execute at a specific block ID  |

## Example

{% tabs %}
{% tab title="cURL" %}
{% code overflow="wrap" %}
```bash
export FLOW_REST=https://shared.eu-central-1.getblock.io/<ACCESS-TOKEN>/

curl -X POST "${FLOW_REST}v1/scripts?block_height=sealed" \
  -H "Content-Type: application/json" \
  -d '{"script": "cHViIGZ1biBtYWluKCk6IEludCB7IHJldHVybiAxICsgMSB9", "arguments": []}'
```
{% endcode %}
{% endtab %}

{% tab title="Axios" %}
{% code title="example.js" %}
```javascript
const axios = require('axios');

const response = await axios.post('https://shared.eu-central-1.getblock.io/<ACCESS-TOKEN>/v1/scripts', {
    script: 'cHViIGZ1biBtYWluKCk6IEludCB7IHJldHVybiAxICsgMSB9',
    arguments: []
}, {
    params: { block_height: 'sealed' },
    headers: { 'Content-Type': 'application/json' }
});

console.log(response.data);
```
{% endcode %}
{% endtab %}

{% tab title="Request" %}
{% code title="example.py" %}
```python
import requests

response = requests.post(
    'https://shared.eu-central-1.getblock.io/<ACCESS-TOKEN>/v1/scripts',
    headers={'Content-Type': 'application/json'},
    params={'block_height': 'sealed'},
    json={
        'script': 'cHViIGZ1biBtYWluKCk6IEludCB7IHJldHVybiAxICsgMSB9',
        'arguments': []
    }
)

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

    let response = client
        .post("https://shared.eu-central-1.getblock.io/<ACCESS-TOKEN>/v1/scripts")
        .query(&[("block_height", "sealed")])
        .header("Content-Type", "application/json")
        .json(&json!({
            "script": "cHViIGZ1biBtYWluKCk6IEludCB7IHJldHVybiAxICsgMSB9",
            "arguments": []
        }))
        .send()
        .await?
        .json::<Value>()
        .await?;

    println!("Result: {}", response);
    Ok(())
}
```
{% endcode %}
{% endtab %}
{% endtabs %}

## Response

```json
"eyJ0eXBlIjoiSW50IiwidmFsdWUiOiIyIn0="
```

## Response Fields

| Field  | Type   | Description                              |
| ------ | ------ | ---------------------------------------- |
| (root) | string | Base64-encoded JSON-Cadence return value |

## Use Cases

* **Read-Only Queries**: Run a Cadence script to read chain state
* **Balance & Metadata**: Resolve token balances and NFT metadata via scripts

## Error Handling

| Error                     | Message       | Description                                                 |
| ------------------------- | ------------- | ----------------------------------------------------------- |
| 400 / Bad request         | Bad request   | A path, query, or body parameter was invalid                |
| 404 / Not found           | Not found     | The block, account, transaction, or resource does not exist |
| 403 / RBAC: access denied | Access denied | The GetBlock access token is missing or incorrect           |
