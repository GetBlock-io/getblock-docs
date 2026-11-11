---
description: >-
  Example code for the /chains/main/blocks/{block}/header REST method. Complete
  guide on how to use the /chains/main/blocks/{block}/header REST method in
  GetBlock Web3 documentation
---

# block-header - Tezos

Returns the full header of the given block: level, protocol, predecessor, timestamp, fitness, operations hash, and the baker's signature.

## Endpoint

```http
GET /chains/main/blocks/{block}/header
```

## Path Parameters

| Parameter | Type   | Description                                      |
| --------- | ------ | ------------------------------------------------ |
| block     | string | Block identifier: head, a level, or a block hash |

## Example

{% tabs %}
{% tab title="cURL" %}
{% code overflow="wrap" %}
```bash
export TEZOS_REST=https://shared.eu-central-1.getblock.io/<ACCESS-TOKEN>/

curl "${TEZOS_REST}chains/main/blocks/head/header"
```
{% endcode %}
{% endtab %}

{% tab title="Axios" %}
{% code title="example.js" %}
```javascript
const axios = require('axios');

const response = await axios.get('https://shared.eu-central-1.getblock.io/<ACCESS-TOKEN>/chains/main/blocks/head/header', {
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
    'https://shared.eu-central-1.getblock.io/<ACCESS-TOKEN>/chains/main/blocks/head/header',
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
        .get("https://shared.eu-central-1.getblock.io/<ACCESS-TOKEN>/chains/main/blocks/head/header")
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
    "protocol": "PsParisC...",
    "chain_id": "NetXdQprcVkpaWU",
    "hash": "BMblockhash...",
    "level": 5000000,
    "proto": 20,
    "predecessor": "BMprev...",
    "timestamp": "2025-01-15T12:00:00Z",
    "validation_pass": 4,
    "operations_hash": "LLoa...",
    "fitness": [
        "02",
        "..."
    ],
    "context": "CoV...",
    "signature": "sig..."
}
```

## Response Fields

| Field            | Type   | Description               |
| ---------------- | ------ | ------------------------- |
| level            | number | Block level (height)      |
| predecessor      | string | Previous block hash       |
| timestamp        | string | Block timestamp (RFC3339) |
| operations\_hash | string | Merkle root of operations |
| signature        | string | Baker signature           |

## Use Cases

* **Height Tracking**: Read the current level
* **Header Sync**: Fetch headers for light verification

## Error Handling

| Error                     | Message       | Description                                       |
| ------------------------- | ------------- | ------------------------------------------------- |
| 400 / Bad request         | Bad request   | A path or body parameter was invalid              |
| 404 / Not found           | Not found     | The block, contract, or resource does not exist   |
| 403 / RBAC: access denied | Access denied | The GetBlock access token is missing or incorrect |
