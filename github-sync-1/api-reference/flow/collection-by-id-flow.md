---
description: >-
  Example code for the /v1/collections/{id} REST method. Complete guide on how
  to use the /v1/collections/{id} REST method in GetBlock Web3 documentation
---

# collection-by-id - Flow

Returns a collection by ID. A collection is a batch of transactions grouped by a collection node and referenced from a block's collection guarantees. By default the response lists links to the collection's transactions; use `expand` to include the full transaction bodies.

## Endpoint

```http
GET /v1/collections/{id}
```

## Path Parameters

| Parameter | Type   | Description         |
| --------- | ------ | ------------------- |
| id        | string | Collection ID (hex) |

## Query Parameters

| Parameter | Type   | Description                      |
| --------- | ------ | -------------------------------- |
| expand    | string | Expand transactions              |
| select    | string | Comma-separated fields to return |

## Example

{% tabs %}
{% tab title="cURL" %}
{% code overflow="wrap" %}
```bash
export FLOW_REST=https://shared.eu-central-1.getblock.io/<ACCESS-TOKEN>/

curl "${FLOW_REST}v1/collections/7bc5d4e3f2a1b0c9d8e7f6a5b4c3d2e1f0a9b8c7d6e5f4a3b2c1d0e9f8a7b6c5"
```
{% endcode %}
{% endtab %}

{% tab title="Axios" %}
{% code title="example.js" %}
```javascript
const axios = require('axios');

const response = await axios.get('https://shared.eu-central-1.getblock.io/<ACCESS-TOKEN>/v1/collections/7bc5d4e3f2a1b0c9d8e7f6a5b4c3d2e1f0a9b8c7d6e5f4a3b2c1d0e9f8a7b6c5', {
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
    'https://shared.eu-central-1.getblock.io/<ACCESS-TOKEN>/v1/collections/7bc5d4e3f2a1b0c9d8e7f6a5b4c3d2e1f0a9b8c7d6e5f4a3b2c1d0e9f8a7b6c5',
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
        .get("https://shared.eu-central-1.getblock.io/<ACCESS-TOKEN>/v1/collections/7bc5d4e3f2a1b0c9d8e7f6a5b4c3d2e1f0a9b8c7d6e5f4a3b2c1d0e9f8a7b6c5")
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
{
    "id": "7bc5...",
    "_expandable": {
        "transactions": [
            "/v1/transactions/a1b2..."
        ]
    },
    "_links": {
        "_self": "/v1/collections/7bc5..."
    }
}
```

## Response Fields

| Field                     | Type   | Description                                                |
| ------------------------- | ------ | ---------------------------------------------------------- |
| id                        | string | Collection ID                                              |
| transactions              | array  | Full transactions in the collection (when expanded)        |
| \_expandable.transactions | array  | Links to the collection's transactions (when not expanded) |
| \_links.\_self            | string | Path of this collection resource                           |

## Use Cases

* **Block Decoding**: Resolve a block's collection guarantees into their transactions
* **Transaction Indexing**: List every transaction ID included in a collection

## Error Handling

| Error                     | Message       | Description                                                 |
| ------------------------- | ------------- | ----------------------------------------------------------- |
| 400 / Bad request         | Bad request   | A path, query, or body parameter was invalid                |
| 404 / Not found           | Not found     | The block, account, transaction, or resource does not exist |
| 403 / RBAC: access denied | Access denied | The GetBlock access token is missing or incorrect           |
