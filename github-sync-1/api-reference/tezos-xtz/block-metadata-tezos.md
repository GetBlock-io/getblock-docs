---
description: >-
  Example code for the /chains/main/blocks/{block}/metadata REST method.
  Complete guide on how to use the /chains/main/blocks/{block}/metadata REST
  method in GetBlock Web3 documentation
---

# block-metadata - Tezos

Returns protocol-level metadata for the block: the active and next protocol, baker, consumed gas, voting period, and balance updates (rewards, deposits, burns).

## Endpoint

```http
GET /chains/main/blocks/{block}/metadata
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

curl "${TEZOS_REST}chains/main/blocks/head/metadata"
```
{% endcode %}
{% endtab %}

{% tab title="Axios" %}
{% code title="example.js" %}
```javascript
const axios = require('axios');

const response = await axios.get('https://shared.eu-central-1.getblock.io/<ACCESS-TOKEN>/chains/main/blocks/head/metadata', {
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
    'https://shared.eu-central-1.getblock.io/<ACCESS-TOKEN>/chains/main/blocks/head/metadata',
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
        .get("https://shared.eu-central-1.getblock.io/<ACCESS-TOKEN>/chains/main/blocks/head/metadata")
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
    "next_protocol": "PsParisC...",
    "proposer": "tz1baker...",
    "baker": "tz1baker...",
    "level_info": {
        "level": 5000000,
        "cycle": 800
    },
    "voting_period_info": {
        "voting_period": {
            "kind": "proposal"
        }
    },
    "consumed_milligas": "0",
    "balance_updates": []
}
```

## Response Fields

| Field                | Type   | Description                   |
| -------------------- | ------ | ----------------------------- |
| proposer             | string | Block proposer address        |
| baker                | string | Baker that produced the block |
| level\_info          | object | Level and cycle               |
| voting\_period\_info | object | Current governance period     |
| balance\_updates     | array  | Reward/deposit/burn movements |

## Use Cases

* **Baker Analytics**: Attribute blocks to bakers
* **Reward Accounting**: Read balance updates per block

## Error Handling

| Error                     | Message       | Description                                       |
| ------------------------- | ------------- | ------------------------------------------------- |
| 400 / Bad request         | Bad request   | A path or body parameter was invalid              |
| 404 / Not found           | Not found     | The block, contract, or resource does not exist   |
| 403 / RBAC: access denied | Access denied | The GetBlock access token is missing or incorrect |
