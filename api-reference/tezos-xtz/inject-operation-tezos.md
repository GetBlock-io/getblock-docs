---
description: >-
  Example code for the /injection/operation REST method. Complete guide on how
  to use the /injection/operation REST method in GetBlock Web3 documentation
---

# inject-operation - Tezos

Broadcasts a signed, forged operation to the network and returns its operation hash. This is how a transaction is submitted to Tezos.

## Endpoint

```http
POST /injection/operation
```

## Query Parameters

| Parameter | Type   | Description                        |
| --------- | ------ | ---------------------------------- |
| chain     | string | Target chain, default main         |
| async     | string | Inject asynchronously (true/false) |

## Example

{% tabs %}
{% tab title="cURL" %}
{% code overflow="wrap" %}
```bash
export TEZOS_REST=https://shared.eu-central-1.getblock.io/<ACCESS-TOKEN>/

curl -X POST "${TEZOS_REST}injection/operation" \
  -H "Content-Type: application/json" \
  -d '"ce69c5713db...signedforgedhex..."'
```
{% endcode %}
{% endtab %}

{% tab title="Axios" %}
{% code title="example.js" %}
```javascript
const axios = require('axios');

// The body is a JSON string: the signed operation bytes in hex, wrapped in quotes
const response = await axios.post(
    'https://shared.eu-central-1.getblock.io/<ACCESS-TOKEN>/injection/operation',
    JSON.stringify('ce69c5713db...signedforgedhex...'),
    { headers: { 'Content-Type': 'application/json' } }
);

console.log(response.data);
```
{% endcode %}
{% endtab %}

{% tab title="Request" %}
{% code title="example.py" %}
```python
import requests

# The body is a JSON string: the signed operation bytes in hex
response = requests.post(
    'https://shared.eu-central-1.getblock.io/<ACCESS-TOKEN>/injection/operation',
    headers={'Content-Type': 'application/json'},
    json='ce69c5713db...signedforgedhex...'
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

    // The body is a JSON string: the signed operation bytes in hex
    let response = client
        .post("https://shared.eu-central-1.getblock.io/<ACCESS-TOKEN>/injection/operation")
        .header("Content-Type", "application/json")
        .json(&json!("ce69c5713db...signedforgedhex..."))
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
"oohash..."
```

## Response Fields

| Field  | Type   | Description                              |
| ------ | ------ | ---------------------------------------- |
| (root) | string | Operation hash of the injected operation |

## Use Cases

* **Transaction Submission**: Broadcast a signed operation
* **Payment Rails**: Send XTZ or call contracts programmatically

## Error Handling

| Error                     | Message       | Description                                       |
| ------------------------- | ------------- | ------------------------------------------------- |
| 400 / Bad request         | Bad request   | A path or body parameter was invalid              |
| 404 / Not found           | Not found     | The block, contract, or resource does not exist   |
| 403 / RBAC: access denied | Access denied | The GetBlock access token is missing or incorrect |
