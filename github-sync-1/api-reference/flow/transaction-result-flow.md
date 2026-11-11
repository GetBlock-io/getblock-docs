---
description: >-
  Example code for the /v1/transaction_results/{id} REST method. Complete guide
  on how to use the /v1/transaction_results/{id} REST method in GetBlock Web3
  documentation
---

# transaction-result - Flow

Returns the execution result of a transaction by its ID: status, status code, any error message, computation used, and the events it emitted. Use it to confirm a transaction sealed successfully.

## Endpoint

```http
GET /v1/transaction_results/{id}
```

## Path Parameters

| Parameter | Type   | Description          |
| --------- | ------ | -------------------- |
| id        | string | Transaction ID (hex) |

## Example

{% tabs %}
{% tab title="cURL" %}
{% code overflow="wrap" %}
```bash
export FLOW_REST=https://shared.eu-central-1.getblock.io/<ACCESS-TOKEN>/

curl "${FLOW_REST}v1/transaction_results/a1b2c3d4e5f60718293a4b5c6d7e8f90a1b2c3d4e5f60718293a4b5c6d7e8f90"
```
{% endcode %}
{% endtab %}

{% tab title="Axios" %}
{% code title="example.js" %}
```javascript
const axios = require('axios');

const response = await axios.get('https://shared.eu-central-1.getblock.io/<ACCESS-TOKEN>/v1/transaction_results/a1b2c3d4e5f60718293a4b5c6d7e8f90a1b2c3d4e5f60718293a4b5c6d7e8f90', {
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
    'https://shared.eu-central-1.getblock.io/<ACCESS-TOKEN>/v1/transaction_results/a1b2c3d4e5f60718293a4b5c6d7e8f90a1b2c3d4e5f60718293a4b5c6d7e8f90',
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
        .get("https://shared.eu-central-1.getblock.io/<ACCESS-TOKEN>/v1/transaction_results/a1b2c3d4e5f60718293a4b5c6d7e8f90a1b2c3d4e5f60718293a4b5c6d7e8f90")
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
    "block_id": "3b5f...",
    "status": "Sealed",
    "status_code": 0,
    "error_message": "",
    "computation_used": "123",
    "events": [
        {
            "type": "A.1654653399040a61.FlowToken.TokensWithdrawn",
            "transaction_id": "a1b2...",
            "transaction_index": "0",
            "event_index": "0",
            "payload": "eyJ0eXBlIjoi..."
        }
    ]
}
```

## Response Fields

| Field             | Type   | Description                                       |
| ----------------- | ------ | ------------------------------------------------- |
| status            | string | Pending / Finalized / Executed / Sealed / Expired |
| status\_code      | number | 0 on success, non-zero on error                   |
| error\_message    | string | Failure reason, empty on success                  |
| computation\_used | string | Computation units consumed                        |
| events            | array  | Events emitted, with base64 payloads              |

## Use Cases

* **Confirmation**: Poll until status is Sealed
* **Event Processing**: Read emitted events after execution

## Error Handling

| Error                     | Message       | Description                                                 |
| ------------------------- | ------------- | ----------------------------------------------------------- |
| 400 / Bad request         | Bad request   | A path, query, or body parameter was invalid                |
| 404 / Not found           | Not found     | The block, account, transaction, or resource does not exist |
| 403 / RBAC: access denied | Access denied | The GetBlock access token is missing or incorrect           |
