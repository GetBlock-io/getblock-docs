---
description: >-
  Example code for the /context/contracts/{contract_id}/balance REST method.
  Complete guide on how to use the /context/contracts/{contract_id}/balance REST
  method in GetBlock Web3 documentation
---

# contract-balance - Tezos

Returns the spendable balance of an account or contract in mutez (1 XTZ = 1,000,000 mutez).

## Endpoint

```http
GET /chains/main/blocks/{block}/context/contracts/{contract_id}/balance
```

## Path Parameters

| Parameter    | Type   | Description                                      |
| ------------ | ------ | ------------------------------------------------ |
| block        | string | Block identifier: head, a level, or a block hash |
| contract\_id | string | Account or contract address (tz1…/KT1…)          |

## Example

{% tabs %}
{% tab title="cURL" %}
{% code overflow="wrap" %}
```bash
export TEZOS_REST=https://shared.eu-central-1.getblock.io/<ACCESS-TOKEN>/

curl "${TEZOS_REST}chains/main/blocks/head/context/contracts/tz1burnburnburnburnburnburjAYjjX/balance"
```
{% endcode %}
{% endtab %}

{% tab title="Axios" %}
{% code title="example.js" %}
```javascript
const axios = require('axios');

const response = await axios.get('https://shared.eu-central-1.getblock.io/<ACCESS-TOKEN>/chains/main/blocks/head/context/contracts/tz1burnburnburnburnburnburjAYjjX/balance', {
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
    'https://shared.eu-central-1.getblock.io/<ACCESS-TOKEN>/chains/main/blocks/head/context/contracts/tz1burnburnburnburnburnburjAYjjX/balance',
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
        .get("https://shared.eu-central-1.getblock.io/<ACCESS-TOKEN>/chains/main/blocks/head/context/contracts/tz1burnburnburnburnburnburjAYjjX/balance")
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
"150000000"
```

## Response Fields

| Field  | Type   | Description      |
| ------ | ------ | ---------------- |
| (root) | string | Balance in mutez |

## Use Cases

* **Balance Reads**: Display an account balance in XTZ
* **Payment Verification**: Confirm a balance at a given block

## Error Handling

| Error                     | Message       | Description                                       |
| ------------------------- | ------------- | ------------------------------------------------- |
| 400 / Bad request         | Bad request   | A path or body parameter was invalid              |
| 404 / Not found           | Not found     | The block, contract, or resource does not exist   |
| 403 / RBAC: access denied | Access denied | The GetBlock access token is missing or incorrect |
