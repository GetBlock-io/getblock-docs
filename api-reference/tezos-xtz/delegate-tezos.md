---
description: >-
  Example code for the /chains/main/blocks/{block}/context/delegates/{pkh} REST
  method. Complete guide on how to use the /context/delegates/{pkh} REST method
  in GetBlock Web3 documentation
---

# delegate - Tezos

Returns the full staking state of a delegate (baker): full and staking balance, delegated contracts, frozen deposits, grace period, and whether it is deactivated.

## Endpoint

```http
GET /chains/main/blocks/{block}/context/delegates/{pkh}
```

## Path Parameters

| Parameter | Type   | Description                                      |
| --------- | ------ | ------------------------------------------------ |
| block     | string | Block identifier: head, a level, or a block hash |
| pkh       | string | Delegate public key hash (tz1…/tz2…/tz3…)        |

## Example

{% tabs %}
{% tab title="cURL" %}
{% code overflow="wrap" %}
```bash
export TEZOS_REST=https://shared.eu-central-1.getblock.io/<ACCESS-TOKEN>/

curl "${TEZOS_REST}chains/main/blocks/head/context/delegates/tz1baker00000000000000000000000000"
```
{% endcode %}
{% endtab %}

{% tab title="Axios" %}
{% code title="example.js" %}
```javascript
const axios = require('axios');

const response = await axios.get('https://shared.eu-central-1.getblock.io/<ACCESS-TOKEN>/chains/main/blocks/head/context/delegates/tz1baker00000000000000000000000000', {
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
    'https://shared.eu-central-1.getblock.io/<ACCESS-TOKEN>/chains/main/blocks/head/context/delegates/tz1baker00000000000000000000000000',
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
        .get("https://shared.eu-central-1.getblock.io/<ACCESS-TOKEN>/chains/main/blocks/head/context/delegates/tz1baker00000000000000000000000000")
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
    "full_balance": "4000000000",
    "current_frozen_deposits": "200000000",
    "staking_balance": "8000000000",
    "delegated_contracts": [
        "tz1..."
    ],
    "delegated_balance": "4000000000",
    "deactivated": false,
    "grace_period": 810
}
```

## Response Fields

| Field                | Type    | Description                        |
| -------------------- | ------- | ---------------------------------- |
| full\_balance        | string  | Delegate's own balance (mutez)     |
| staking\_balance     | string  | Own + delegated stake (mutez)      |
| delegated\_contracts | array   | Addresses delegating to this baker |
| deactivated          | boolean | Whether the delegate is inactive   |

## Use Cases

* **Staking Analytics**: Measure a baker's stake and delegators
* **Delegation UX**: Show baker health before delegating

## Error Handling

| Error                     | Message       | Description                                       |
| ------------------------- | ------------- | ------------------------------------------------- |
| 400 / Bad request         | Bad request   | A path or body parameter was invalid              |
| 404 / Not found           | Not found     | The block, contract, or resource does not exist   |
| 403 / RBAC: access denied | Access denied | The GetBlock access token is missing or incorrect |
