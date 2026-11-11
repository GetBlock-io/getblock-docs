---
description: >-
  Example code for the /chains/main/blocks/{block}/votes/current_period REST
  method. Complete guide on how to use the /votes/current_period REST method in
  GetBlock Web3 documentation
---

# votes-current-period - Tezos

Returns the current governance (amendment) voting period: its kind (proposal, exploration, cooldown, promotion, adoption), index, and start position. Tezos self-amends through these on-chain periods.

## Endpoint

```http
GET /chains/main/blocks/{block}/votes/current_period
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

curl "${TEZOS_REST}chains/main/blocks/head/votes/current_period"
```
{% endcode %}
{% endtab %}

{% tab title="Axios" %}
{% code title="example.js" %}
```javascript
const axios = require('axios');

const response = await axios.get('https://shared.eu-central-1.getblock.io/<ACCESS-TOKEN>/chains/main/blocks/head/votes/current_period', {
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
    'https://shared.eu-central-1.getblock.io/<ACCESS-TOKEN>/chains/main/blocks/head/votes/current_period',
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
        .get("https://shared.eu-central-1.getblock.io/<ACCESS-TOKEN>/chains/main/blocks/head/votes/current_period")
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
    "voting_period": {
        "index": 120,
        "kind": "proposal",
        "start_position": 2949120
    },
    "position": 1024,
    "remaining": 23551
}
```

## Response Fields

| Field                | Type   | Description                                              |
| -------------------- | ------ | -------------------------------------------------------- |
| voting\_period.kind  | string | proposal / exploration / cooldown / promotion / adoption |
| voting\_period.index | number | Sequential period index                                  |
| remaining            | number | Blocks remaining in the period                           |

## Use Cases

* **Governance Tracking**: Follow the on-chain amendment process
* **Voting UX**: Show the active period and time remaining

## Error Handling

| Error                     | Message       | Description                                       |
| ------------------------- | ------------- | ------------------------------------------------- |
| 400 / Bad request         | Bad request   | A path or body parameter was invalid              |
| 404 / Not found           | Not found     | The block, contract, or resource does not exist   |
| 403 / RBAC: access denied | Access denied | The GetBlock access token is missing or incorrect |
