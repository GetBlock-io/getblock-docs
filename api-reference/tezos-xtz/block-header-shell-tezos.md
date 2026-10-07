---
description: >-
  Example code for the /chains/main/blocks/{block}/header/shell REST method.
  Complete guide on how to use the /chains/main/blocks/{block}/header/shell REST
  method in GetBlock Web3 documentation
---

# block-header-shell - Tezos

Returns the protocol-agnostic shell portion of the block header — level, predecessor, timestamp, fitness, and operations hash — without protocol-specific fields.

## Endpoint

```http
GET /chains/main/blocks/{block}/header/shell
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

curl "${TEZOS_REST}chains/main/blocks/head/header/shell"
```
{% endcode %}
{% endtab %}

{% tab title="Axios" %}
{% code title="example.js" %}
```javascript
const axios = require('axios');

const response = await axios.get('https://shared.eu-central-1.getblock.io/<ACCESS-TOKEN>/chains/main/blocks/head/header/shell', {
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
    'https://shared.eu-central-1.getblock.io/<ACCESS-TOKEN>/chains/main/blocks/head/header/shell',
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
        .get("https://shared.eu-central-1.getblock.io/<ACCESS-TOKEN>/chains/main/blocks/head/header/shell")
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
    "context": "CoV..."
}
```

## Response Fields

| Field       | Type   | Description         |
| ----------- | ------ | ------------------- |
| level       | number | Block level         |
| predecessor | string | Previous block hash |
| fitness     | array  | Chain fitness       |
| context     | string | Context hash        |

## Use Cases

* **Light Clients**: Read the shell header without protocol decoding
* **Fitness Checks**: Compare chain fitness

## Error Handling

| Error                     | Message       | Description                                       |
| ------------------------- | ------------- | ------------------------------------------------- |
| 400 / Bad request         | Bad request   | A path or body parameter was invalid              |
| 404 / Not found           | Not found     | The block, contract, or resource does not exist   |
| 403 / RBAC: access denied | Access denied | The GetBlock access token is missing or incorrect |
