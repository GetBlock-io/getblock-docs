---
description: >-
  Example code for the /v1/blocks/{id} REST method. Complete guide on how to use
  the /v1/blocks/{id} REST method in GetBlock Web3 documentation
---

# block-by-id - Flow

Returns one or more blocks by block ID. Accepts a single ID or a comma-separated list, and the same expand/select options as the height query.

## Endpoint

```http
GET /v1/blocks/{id}
```

## Path Parameters

| Parameter | Type   | Description                                      |
| --------- | ------ | ------------------------------------------------ |
| id        | string | Block ID (hex), or a comma-separated list of IDs |

## Query Parameters

| Parameter | Type   | Description                             |
| --------- | ------ | --------------------------------------- |
| expand    | string | Expand payload and/or execution\_result |
| select    | string | Comma-separated fields to return        |

## Example

{% tabs %}
{% tab title="cURL" %}
{% code overflow="wrap" %}
```bash
export FLOW_REST=https://shared.eu-central-1.getblock.io/<ACCESS-TOKEN>/

curl "${FLOW_REST}v1/blocks/3b5f0d8a1c2e4f6072839a1b2c3d4e5f60718293a4b5c6d7e8f901a2b3c4d5e6"
```
{% endcode %}
{% endtab %}

{% tab title="Axios" %}
{% code title="example.js" %}
```javascript
const axios = require('axios');

const response = await axios.get('https://shared.eu-central-1.getblock.io/<ACCESS-TOKEN>/v1/blocks/3b5f0d8a1c2e4f6072839a1b2c3d4e5f60718293a4b5c6d7e8f901a2b3c4d5e6', {
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
    'https://shared.eu-central-1.getblock.io/<ACCESS-TOKEN>/v1/blocks/3b5f0d8a1c2e4f6072839a1b2c3d4e5f60718293a4b5c6d7e8f901a2b3c4d5e6',
    headers={'Content-Type': 'application/json'}
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
        .get("https://shared.eu-central-1.getblock.io/<ACCESS-TOKEN>/v1/blocks/3b5f0d8a1c2e4f6072839a1b2c3d4e5f60718293a4b5c6d7e8f901a2b3c4d5e6")
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
        }
    }
]
```

## Response Fields

| Field             | Type   | Description     |
| ----------------- | ------ | --------------- |
| header.id         | string | Block ID        |
| header.parent\_id | string | Parent block ID |
| header.height     | string | Block height    |

## Use Cases

* **Block Lookup**: Fetch a specific block by hash
* **Fork Inspection**: Compare blocks by ID

## Error Handling

| Error                     | Message       | Description                                                 |
| ------------------------- | ------------- | ----------------------------------------------------------- |
| 400 / Bad request         | Bad request   | A path, query, or body parameter was invalid                |
| 404 / Not found           | Not found     | The block, account, transaction, or resource does not exist |
| 403 / RBAC: access denied | Access denied | The GetBlock access token is missing or incorrect           |
