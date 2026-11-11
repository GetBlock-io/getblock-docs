---
description: >-
  Example code for the /chains/main/blocks/{block}/operations REST method.
  Complete guide on how to use the /chains/main/blocks/{block}/operations REST
  method in GetBlock Web3 documentation
---

# block-operations - Tezos

Returns all operations included in the block, grouped by validation pass (endorsements, votes, anonymous, and managers). Manager operations include transactions, originations, delegations, and smart-contract calls.

## Endpoint

```http
GET /chains/main/blocks/{block}/operations
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

curl "${TEZOS_REST}chains/main/blocks/head/operations"
```
{% endcode %}
{% endtab %}

{% tab title="Axios" %}
{% code title="example.js" %}
```javascript
const axios = require('axios');

const response = await axios.get('https://shared.eu-central-1.getblock.io/<ACCESS-TOKEN>/chains/main/blocks/head/operations', {
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
    'https://shared.eu-central-1.getblock.io/<ACCESS-TOKEN>/chains/main/blocks/head/operations',
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
        .get("https://shared.eu-central-1.getblock.io/<ACCESS-TOKEN>/chains/main/blocks/head/operations")
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
    [
        {
            "protocol": "PsParisC...",
            "chain_id": "NetXdQprcVkpaWU",
            "hash": "oohash...",
            "contents": [
                {
                    "kind": "attestation",
                    "level": 4999999
                }
            ]
        }
    ]
]
```

## Response Fields

| Field    | Type   | Description                          |
| -------- | ------ | ------------------------------------ |
| (root)   | array  | Four arrays, one per validation pass |
| contents | array  | Decoded operation contents           |
| hash     | string | Operation hash                       |

## Use Cases

* **Transaction Indexing**: Extract transfers and contract calls from a block
* **Governance**: Read ballots and proposals

## Error Handling

| Error                     | Message       | Description                                       |
| ------------------------- | ------------- | ------------------------------------------------- |
| 400 / Bad request         | Bad request   | A path or body parameter was invalid              |
| 404 / Not found           | Not found     | The block, contract, or resource does not exist   |
| 403 / RBAC: access denied | Access denied | The GetBlock access token is missing or incorrect |
