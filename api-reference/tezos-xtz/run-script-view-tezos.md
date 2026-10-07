---
description: >-
  Example code for the /helpers/scripts/run_script_view REST method. Complete
  guide on how to use the /helpers/scripts/run_script_view REST method in
  GetBlock Web3 documentation
---

# run-script-view - Tezos

Executes an on-chain view of a smart contract and returns its result, without creating a transaction. Views are read-only contract functions.

## Endpoint

```http
POST /chains/main/blocks/{block}/helpers/scripts/run_script_view
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

curl -X POST "${TEZOS_REST}chains/main/blocks/head/helpers/scripts/run_script_view" \
  -H "Content-Type: application/json" \
  -d '{"contract": "KT1BuEZtb68c1Q4yjtckcNjGELqWt56Xyesc", "view": "get_balance", "input": {"string": "tz1..."}, "chain_id": "NetXdQprcVkpaWU", "unparsing_mode": "Readable"}'
```
{% endcode %}
{% endtab %}

{% tab title="Axios" %}
{% code title="example.js" %}
```javascript
const axios = require('axios');

const response = await axios.post('https://shared.eu-central-1.getblock.io/<ACCESS-TOKEN>/chains/main/blocks/head/helpers/scripts/run_script_view', {
    contract: 'KT1BuEZtb68c1Q4yjtckcNjGELqWt56Xyesc',
    view: 'get_balance',
    input: { string: 'tz1...' },
    chain_id: 'NetXdQprcVkpaWU',
    unparsing_mode: 'Readable'
}, {
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

response = requests.post(
    'https://shared.eu-central-1.getblock.io/<ACCESS-TOKEN>/chains/main/blocks/head/helpers/scripts/run_script_view',
    headers={'Content-Type': 'application/json'},
    json={
        'contract': 'KT1BuEZtb68c1Q4yjtckcNjGELqWt56Xyesc',
        'view': 'get_balance',
        'input': {'string': 'tz1...'},
        'chain_id': 'NetXdQprcVkpaWU',
        'unparsing_mode': 'Readable'
    }
)

print(response.json())
```
{% endcode %}
{% endtab %}

{% tab title="Rust" %}
{% code title="example.rs" %}
```rust
use reqwest::Client;
use serde_json::{json, Value};

#[tokio::main]
async fn main() -> Result<(), Box<dyn std::error::Error>> {
    let client = Client::new();

    let response = client
        .post("https://shared.eu-central-1.getblock.io/<ACCESS-TOKEN>/chains/main/blocks/head/helpers/scripts/run_script_view")
        .header("Content-Type", "application/json")
        .json(&json!({
            "contract": "KT1BuEZtb68c1Q4yjtckcNjGELqWt56Xyesc",
            "view": "get_balance",
            "input": { "string": "tz1..." },
            "chain_id": "NetXdQprcVkpaWU",
            "unparsing_mode": "Readable"
        }))
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
    "data": {
        "int": "1000000"
    }
}
```

## Response Fields

| Field | Type   | Description                                       |
| ----- | ------ | ------------------------------------------------- |
| data  | object | The view's return value as a Micheline expression |

## Use Cases

* **Read-Only Calls**: Query a contract view without a transaction
* **Token Metadata**: Resolve FA2 balances/metadata via views

## Error Handling

| Error                     | Message       | Description                                       |
| ------------------------- | ------------- | ------------------------------------------------- |
| 400 / Bad request         | Bad request   | A path or body parameter was invalid              |
| 404 / Not found           | Not found     | The block, contract, or resource does not exist   |
| 403 / RBAC: access denied | Access denied | The GetBlock access token is missing or incorrect |
