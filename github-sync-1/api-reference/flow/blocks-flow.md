---
description: >-
  Example code for the /v1/blocks REST method. Complete guide on how to use the
  /v1/blocks REST method in GetBlock Web3 documentation
---

# blocks - Flow

Returns one or more blocks by height. Use the `height` query parameter with a number, the special values `final` or `sealed`, or a `start_height`/`end_height` range. Blocks can be expanded to include payload and execution result.

## Endpoint

```http
GET /v1/blocks
```

## Query Parameters

| Parameter     | Type   | Description                                                                    |
| ------------- | ------ | ------------------------------------------------------------------------------ |
| height        | string | Block height, or the keywords `final` or `sealed`; comma-separated for several |
| start\_height | string | Start of a height range                                                        |
| end\_height   | string | End of a height range                                                          |
| expand        | string | Expand `payload` and/or `execution_result`                                     |
| select        | string | Comma-separated fields to return                                               |

## Example

{% tabs %}
{% tab title="cURL" %}
{% code overflow="wrap" %}
```bash
export FLOW_REST=https://shared.eu-central-1.getblock.io/<ACCESS-TOKEN>/

curl "${FLOW_REST}v1/blocks?height=sealed"
```
{% endcode %}
{% endtab %}

{% tab title="Axios" %}
{% code title="example.js" %}
```javascript
const axios = require('axios');

const response = await axios.get('https://shared.eu-central-1.getblock.io/<ACCESS-TOKEN>/v1/blocks', {
    params: { height: 'sealed' },
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

response = requests.get(
    'https://shared.eu-central-1.getblock.io/<ACCESS-TOKEN>/v1/blocks',
    headers={'Content-Type': 'application/json'},
    params={'height': 'sealed'}
)

print(response.json())
```
{% endcode %}
{% endtab %}

{% tab title="Rust" %}
{% code title="example.rs" %}
```rust
use reqwest::Client;
use serde_json::Value;

#[tokio::main]
async fn main() -> Result<(), Box<dyn std::error::Error>> {
    let client = Client::new();

    let response = client
        .get("https://shared.eu-central-1.getblock.io/<ACCESS-TOKEN>/v1/blocks")
        .query(&[("height", "sealed")])
        .header("Content-Type", "application/json")
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
[
    {
        "header": {
            "id": "3b5f...",
            "parent_id": "9a1c...",
            "height": "88226300",
            "timestamp": "2025-01-15T12:00:00Z"
        },
        "payload": {
            "collection_guarantees": [],
            "block_seals": []
        },
        "_expandable": {
            "payload": "",
            "execution_result": ""
        }
    }
]
```

## Response Fields

| Field            | Type   | Description                           |
| ---------------- | ------ | ------------------------------------- |
| header.id        | string | Block ID (hex)                        |
| header.height    | string | Block height                          |
| header.timestamp | string | Block timestamp (RFC3339)             |
| payload          | object | Collection guarantees and block seals |

## Use Cases

* **Chain Head**: Read the latest sealed block
* **Range Sync**: Page a height range for indexing

## Error Handling

| Error                     | Message       | Description                                                 |
| ------------------------- | ------------- | ----------------------------------------------------------- |
| 400 / Bad request         | Bad request   | A path, query, or body parameter was invalid                |
| 404 / Not found           | Not found     | The block, account, transaction, or resource does not exist |
| 403 / RBAC: access denied | Access denied | The GetBlock access token is missing or incorrect           |
