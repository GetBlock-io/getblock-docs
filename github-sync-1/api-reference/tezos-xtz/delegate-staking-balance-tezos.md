---
description: >-
  Example code for the /context/delegates/{pkh}/staking_balance REST method.
  Complete guide on how to use the /context/delegates/{pkh}/staking_balance REST
  method in GetBlock Web3 documentation
---

# delegate-staking-balance - Tezos

Returns a delegate's total staking balance in mutez — its own stake plus all balances delegated to it — which determines baking and attestation rights.

## Endpoint

```http
GET /chains/main/blocks/{block}/context/delegates/{pkh}/staking_balance
```

## Path Parameters

| Parameter | Type   | Description                                      |
| --------- | ------ | ------------------------------------------------ |
| block     | string | Block identifier: head, a level, or a block hash |
| pkh       | string | Delegate public key hash (tz1…)                  |

## Example

{% tabs %}
{% tab title="cURL" %}
{% code overflow="wrap" %}
```bash
export TEZOS_REST=https://shared.eu-central-1.getblock.io/<ACCESS-TOKEN>/

curl "${TEZOS_REST}chains/main/blocks/head/context/delegates/tz1baker00000000000000000000000000/staking_balance"
```
{% endcode %}
{% endtab %}

{% tab title="Axios" %}
{% code title="example.js" %}
```javascript
const axios = require('axios');

const response = await axios.get('https://shared.eu-central-1.getblock.io/<ACCESS-TOKEN>/chains/main/blocks/head/context/delegates/tz1baker00000000000000000000000000/staking_balance', {
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
    'https://shared.eu-central-1.getblock.io/<ACCESS-TOKEN>/chains/main/blocks/head/context/delegates/tz1baker00000000000000000000000000/staking_balance',
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
        .get("https://shared.eu-central-1.getblock.io/<ACCESS-TOKEN>/chains/main/blocks/head/context/delegates/tz1baker00000000000000000000000000/staking_balance")
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
"8000000000"
```

## Response Fields

| Field  | Type   | Description                    |
| ------ | ------ | ------------------------------ |
| (root) | string | Total staking balance in mutez |

## Use Cases

* **Rights Estimation**: Gauge a baker's share of consensus
* **Reward Modelling**: Project staking rewards

## Error Handling

| Error                     | Message       | Description                                       |
| ------------------------- | ------------- | ------------------------------------------------- |
| 400 / Bad request         | Bad request   | A path or body parameter was invalid              |
| 404 / Not found           | Not found     | The block, contract, or resource does not exist   |
| 403 / RBAC: access denied | Access denied | The GetBlock access token is missing or incorrect |
