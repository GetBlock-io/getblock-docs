---
description: >-
  Example code for the /v1/execution_results REST method. Complete guide on how
  to use the /v1/execution_results REST method in GetBlock Web3 documentation
---

# execution-results - Flow

Returns the execution results for one or more blocks, selected by block ID. An execution result commits to the computed state after a block, including chunks and the result's own ID.

## Endpoint

```http
GET /v1/execution_results
```

## Query Parameters

| Parameter | Type   | Description                          |
| --------- | ------ | ------------------------------------ |
| block\_id | string | Comma-separated block IDs (required) |

## Example

{% tabs %}
{% tab title="cURL" %}
{% code overflow="wrap" %}
```bash
export FLOW_REST=https://shared.eu-central-1.getblock.io/<ACCESS-TOKEN>/

curl "${FLOW_REST}v1/execution_results?block_id=3b5f0d8a1c2e4f6072839a1b2c3d4e5f60718293a4b5c6d7e8f901a2b3c4d5e6"
```
{% endcode %}
{% endtab %}

{% tab title="Axios" %}
{% code title="example.js" %}
```javascript
const axios = require('axios');

const response = await axios.get('https://shared.eu-central-1.getblock.io/<ACCESS-TOKEN>/v1/execution_results', {
    params: { block_id: '3b5f0d8a1c2e4f6072839a1b2c3d4e5f60718293a4b5c6d7e8f901a2b3c4d5e6' },
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
    'https://shared.eu-central-1.getblock.io/<ACCESS-TOKEN>/v1/execution_results',
    headers={'Content-Type': 'application/json'},
    params={'block_id': '3b5f0d8a1c2e4f6072839a1b2c3d4e5f60718293a4b5c6d7e8f901a2b3c4d5e6'}
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
        .get("https://shared.eu-central-1.getblock.io/<ACCESS-TOKEN>/v1/execution_results")
        .query(&[("block_id", "3b5f0d8a1c2e4f6072839a1b2c3d4e5f60718293a4b5c6d7e8f901a2b3c4d5e6")])
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
]
```

## Response Fields

| Field     | Type   | Description                                            |
| --------- | ------ | ------------------------------------------------------ |
| id        | string | Execution result ID                                    |
| block\_id | string | Block this result commits to                           |
| chunks    | array  | Per-collection execution chunks with state commitments |

## Use Cases

* **State Verification**: Read the committed state result of a block
* **Chunk Analysis**: Inspect per-collection execution

## Error Handling

| Error                     | Message       | Description                                                 |
| ------------------------- | ------------- | ----------------------------------------------------------- |
| 400 / Bad request         | Bad request   | A path, query, or body parameter was invalid                |
| 404 / Not found           | Not found     | The block, account, transaction, or resource does not exist |
| 403 / RBAC: access denied | Access denied | The GetBlock access token is missing or incorrect           |
