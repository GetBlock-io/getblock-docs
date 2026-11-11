---
description: >-
  Example code for the /context/constants REST method. Complete guide on how to
  use the /context/constants REST method in GetBlock Web3 documentation
---

# constants - Tezos

Returns the active protocol's constants — block time, blocks per cycle, minimal stake, hard gas and storage limits, and reward parameters. These govern how the chain behaves under the current protocol.

## Endpoint

```http
GET /chains/main/blocks/{block}/context/constants
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

curl "${TEZOS_REST}chains/main/blocks/head/context/constants"
```
{% endcode %}
{% endtab %}

{% tab title="Axios" %}
{% code title="example.js" %}
```javascript
const axios = require('axios');

const response = await axios.get('https://shared.eu-central-1.getblock.io/<ACCESS-TOKEN>/chains/main/blocks/head/context/constants', {
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
    'https://shared.eu-central-1.getblock.io/<ACCESS-TOKEN>/chains/main/blocks/head/context/constants',
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
        .get("https://shared.eu-central-1.getblock.io/<ACCESS-TOKEN>/chains/main/blocks/head/context/constants")
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
    "proof_of_work_nonce_size": 8,
    "blocks_per_cycle": 24576,
    "hard_gas_limit_per_operation": "1040000",
    "hard_gas_limit_per_block": "1733333",
    "hard_storage_limit_per_operation": "60000",
    "cost_per_byte": "250",
    "minimal_block_delay": "8"
}
```

## Response Fields

| Field                            | Type   | Description                   |
| -------------------------------- | ------ | ----------------------------- |
| blocks\_per\_cycle               | number | Blocks in a cycle             |
| hard\_gas\_limit\_per\_operation | string | Max gas per operation         |
| cost\_per\_byte                  | string | Storage burn per byte (mutez) |
| minimal\_block\_delay            | string | Target block time in seconds  |

## Use Cases

* **Fee Estimation**: Read gas/storage limits and burn costs
* **Protocol Awareness**: Adapt to the active protocol's parameters

## Error Handling

| Error                     | Message       | Description                                       |
| ------------------------- | ------------- | ------------------------------------------------- |
| 400 / Bad request         | Bad request   | A path or body parameter was invalid              |
| 404 / Not found           | Not found     | The block, contract, or resource does not exist   |
| 403 / RBAC: access denied | Access denied | The GetBlock access token is missing or incorrect |
