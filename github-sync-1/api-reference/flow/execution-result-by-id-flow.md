---
description: >-
  Example code for the /v1/execution_results/{id} REST method. Complete guide on
  how to use the /v1/execution_results/{id} REST method in GetBlock Web3
  documentation
---

# execution-result-by-id - Flow

Returns a single execution result by its own result ID (not the block ID), including its chunks and the block it commits to.

## Endpoint

```http
GET /v1/execution_results/{id}
```

## Path Parameters

| Parameter | Type   | Description               |
| --------- | ------ | ------------------------- |
| id        | string | Execution result ID (hex) |

## Example

{% tabs %}
{% tab title="cURL" %}
{% code overflow="wrap" %}
```bash
export FLOW_REST=https://shared.eu-central-1.getblock.io/<ACCESS-TOKEN>/

curl "${FLOW_REST}v1/execution_results/c3d4e5f60718293a4b5c6d7e8f90a1b2c3d4e5f60718293a4b5c6d7e8f90a1b2"
```
{% endcode %}
{% endtab %}

{% tab title="Axios" %}
{% code title="example.js" %}
```javascript
const axios = require('axios');

const response = await axios.get('https://shared.eu-central-1.getblock.io/<ACCESS-TOKEN>/v1/execution_results/c3d4e5f60718293a4b5c6d7e8f90a1b2c3d4e5f60718293a4b5c6d7e8f90a1b2', {
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
    'https://shared.eu-central-1.getblock.io/<ACCESS-TOKEN>/v1/execution_results/c3d4e5f60718293a4b5c6d7e8f90a1b2c3d4e5f60718293a4b5c6d7e8f90a1b2',
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
        .get("https://shared.eu-central-1.getblock.io/<ACCESS-TOKEN>/v1/execution_results/c3d4e5f60718293a4b5c6d7e8f90a1b2c3d4e5f60718293a4b5c6d7e8f90a1b2")
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
    "id": "c3d4...",
    "block_id": "3b5f...",
    "previous_result_id": "b2c3...",
    "chunks": [
        {
            "start_state": "...",
            "end_state": "...",
            "number_of_transactions": "3"
        }
    ]
}
```

## Response Fields

| Field                | Type   | Description                       |
| -------------------- | ------ | --------------------------------- |
| id                   | string | Execution result ID               |
| block\_id            | string | Block this result commits to      |
| previous\_result\_id | string | Prior block's execution result ID |

## Use Cases

* **Result Lookup**: Fetch an execution result directly by its ID
* **Verification Pipelines**: Chain results via `previous_result_id`

## Error Handling

| Error                     | Message       | Description                                                 |
| ------------------------- | ------------- | ----------------------------------------------------------- |
| 400 / Bad request         | Bad request   | A path, query, or body parameter was invalid                |
| 404 / Not found           | Not found     | The block, account, transaction, or resource does not exist |
| 403 / RBAC: access denied | Access denied | The GetBlock access token is missing or incorrect           |
