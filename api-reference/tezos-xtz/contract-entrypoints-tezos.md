---
description: >-
  Example code for the /context/contracts/{contract_id}/entrypoints REST method.
  Complete guide on how to use the /context/contracts/{contract_id}/entrypoints
  REST method in GetBlock Web3 documentation
---

# contract-entrypoints - Tezos

Returns the named entrypoints of an originated (KT1) contract and their parameter types, so a caller knows how to build a valid transaction to the contract.

## Endpoint

```http
GET /chains/main/blocks/{block}/context/contracts/{contract_id}/entrypoints
```

## Path Parameters

| Parameter    | Type   | Description                                      |
| ------------ | ------ | ------------------------------------------------ |
| block        | string | Block identifier: head, a level, or a block hash |
| contract\_id | string | Originated contract address (KT1…)               |

## Example

{% tabs %}
{% tab title="cURL" %}
{% code overflow="wrap" %}
```bash
export TEZOS_REST=https://shared.eu-central-1.getblock.io/<ACCESS-TOKEN>/

curl "${TEZOS_REST}chains/main/blocks/head/context/contracts/KT1BuEZtb68c1Q4yjtckcNjGELqWt56Xyesc/entrypoints"
```
{% endcode %}
{% endtab %}

{% tab title="Axios" %}
{% code title="example.js" %}
```javascript
const axios = require('axios');

const response = await axios.get('https://shared.eu-central-1.getblock.io/<ACCESS-TOKEN>/chains/main/blocks/head/context/contracts/KT1BuEZtb68c1Q4yjtckcNjGELqWt56Xyesc/entrypoints', {
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
    'https://shared.eu-central-1.getblock.io/<ACCESS-TOKEN>/chains/main/blocks/head/context/contracts/KT1BuEZtb68c1Q4yjtckcNjGELqWt56Xyesc/entrypoints',
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
        .get("https://shared.eu-central-1.getblock.io/<ACCESS-TOKEN>/chains/main/blocks/head/context/contracts/KT1BuEZtb68c1Q4yjtckcNjGELqWt56Xyesc/entrypoints")
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
    "entrypoints": {
        "transfer": {
            "prim": "pair",
            "args": [
                {
                    "prim": "address"
                },
                {
                    "prim": "nat"
                }
            ]
        },
        "approve": {
            "prim": "pair"
        }
    }
}
```

## Response Fields

| Field       | Type   | Description                              |
| ----------- | ------ | ---------------------------------------- |
| entrypoints | object | Map of entrypoint name to parameter type |

## Use Cases

* **Contract Calls**: Discover callable entrypoints and their types
* **Wallet UX**: Render a form from entrypoint signatures

## Error Handling

| Error                     | Message       | Description                                       |
| ------------------------- | ------------- | ------------------------------------------------- |
| 400 / Bad request         | Bad request   | A path or body parameter was invalid              |
| 404 / Not found           | Not found     | The block, contract, or resource does not exist   |
| 403 / RBAC: access denied | Access denied | The GetBlock access token is missing or incorrect |
