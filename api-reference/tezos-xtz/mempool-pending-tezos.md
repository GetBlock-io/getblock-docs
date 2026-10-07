---
description: >-
  Example code for the /chains/main/mempool/pending_operations REST method.
  Complete guide on how to use the /chains/main/mempool/pending_operations REST
  method in GetBlock Web3 documentation
---

# mempool-pending - Tezos

Returns operations currently in the node's mempool, classified as applied, refused, branch\_refused, branch\_delayed, or outdated. Use it to watch unconfirmed transactions.

## Endpoint

```http
GET /chains/main/mempool/pending_operations
```

## Query Parameters

| Parameter | Type   | Description                               |
| --------- | ------ | ----------------------------------------- |
| validated | string | Include validated operations (true/false) |

## Example

{% tabs %}
{% tab title="cURL" %}
{% code overflow="wrap" %}
```bash
export TEZOS_REST=https://shared.eu-central-1.getblock.io/<ACCESS-TOKEN>/

curl "${TEZOS_REST}chains/main/mempool/pending_operations"
```
{% endcode %}
{% endtab %}

{% tab title="Axios" %}
{% code title="example.js" %}
```javascript
const axios = require('axios');

const response = await axios.get('https://shared.eu-central-1.getblock.io/<ACCESS-TOKEN>/chains/main/mempool/pending_operations', {
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
    'https://shared.eu-central-1.getblock.io/<ACCESS-TOKEN>/chains/main/mempool/pending_operations',
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
        .get("https://shared.eu-central-1.getblock.io/<ACCESS-TOKEN>/chains/main/mempool/pending_operations")
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
    "validated": [
        {
            "hash": "oohash...",
            "contents": [
                {
                    "kind": "transaction",
                    "amount": "1000000"
                }
            ]
        }
    ],
    "refused": [],
    "branch_refused": [],
    "branch_delayed": [],
    "outdated": []
}
```

## Response Fields

| Field           | Type  | Description                          |
| --------------- | ----- | ------------------------------------ |
| validated       | array | Operations accepted into the mempool |
| refused         | array | Permanently rejected operations      |
| branch\_delayed | array | Operations waiting on a branch       |

## Use Cases

* **Pending Tracking**: Watch a transaction before it is baked
* **Fee Market**: Observe mempool pressure

## Error Handling

| Error                     | Message       | Description                                       |
| ------------------------- | ------------- | ------------------------------------------------- |
| 400 / Bad request         | Bad request   | A path or body parameter was invalid              |
| 404 / Not found           | Not found     | The block, contract, or resource does not exist   |
| 403 / RBAC: access denied | Access denied | The GetBlock access token is missing or incorrect |
