---
description: >-
  Example code for the /context/big_maps/{big_map_id}/{script_expr} REST method.
  Complete guide on how to use the /context/big_maps/{big_map_id}/{script_expr}
  REST method in GetBlock Web3 documentation
---

# big\_map\_value - Tezos

Returns a single value from a big map by its packed-key hash (script expression). Big maps hold large key-value stores such as token ledgers.

## Endpoint

```http
GET /chains/main/blocks/{block}/context/big_maps/{big_map_id}/{script_expr}
```

## Path Parameters

| Parameter    | Type   | Description                                      |
| ------------ | ------ | ------------------------------------------------ |
| block        | string | Block identifier: head, a level, or a block hash |
| big\_map\_id | string | Numeric big map identifier                       |
| script\_expr | string | Script expression hash of the packed key (expr…) |

## Example

{% tabs %}
{% tab title="cURL" %}
{% code overflow="wrap" %}
```bash
export TEZOS_REST=https://shared.eu-central-1.getblock.io/<ACCESS-TOKEN>/

curl "${TEZOS_REST}chains/main/blocks/head/context/big_maps/5696/exprvValue..."
```
{% endcode %}
{% endtab %}

{% tab title="Axios" %}
{% code title="example.js" %}
```javascript
const axios = require('axios');

const response = await axios.get('https://shared.eu-central-1.getblock.io/<ACCESS-TOKEN>/chains/main/blocks/head/context/big_maps/5696/exprvValue...', {
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
    'https://shared.eu-central-1.getblock.io/<ACCESS-TOKEN>/chains/main/blocks/head/context/big_maps/5696/exprvValue...',
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
        .get("https://shared.eu-central-1.getblock.io/<ACCESS-TOKEN>/chains/main/blocks/head/context/big_maps/5696/exprvValue...")
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
    "prim": "Pair",
    "args": [
        {
            "int": "1000000"
        },
        []
    ]
}
```

## Response Fields

| Field  | Type   | Description                             |
| ------ | ------ | --------------------------------------- |
| (root) | object | Big map value as a Micheline expression |

## Use Cases

* **Token Balances**: Read an FA1.2/FA2 ledger entry
* **dApp State**: Look up a specific big-map key

## Error Handling

| Error                     | Message       | Description                                       |
| ------------------------- | ------------- | ------------------------------------------------- |
| 400 / Bad request         | Bad request   | A path or body parameter was invalid              |
| 404 / Not found           | Not found     | The block, contract, or resource does not exist   |
| 403 / RBAC: access denied | Access denied | The GetBlock access token is missing or incorrect |
