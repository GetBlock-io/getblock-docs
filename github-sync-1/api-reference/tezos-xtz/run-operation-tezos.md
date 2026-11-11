---
description: >-
  Example code for the /chains/main/blocks/{block}/helpers/scripts/run_operation
  REST method. Complete guide on how to use the /helpers/scripts/run_operation
  REST method in GetBlock Web3 documentation
---

# run-operation - Tezos

Simulates (dry-runs) a signed operation against the current context without broadcasting it, returning the operation results including consumed gas and any errors. Use it to validate and size an operation before injection.

## Endpoint

```http
POST /chains/main/blocks/{block}/helpers/scripts/run_operation
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

curl -X POST "${TEZOS_REST}chains/main/blocks/head/helpers/scripts/run_operation" \
  -H "Content-Type: application/json" \
  -d '{"operation": {"branch": "BMbranch...", "contents": [{"kind": "transaction", "source": "tz1...", "fee": "500", "counter": "12346", "gas_limit": "1040000", "storage_limit": "300", "amount": "1000000", "destination": "tz1dest..."}], "signature": "sig..."}, "chain_id": "NetXdQprcVkpaWU"}'
```
{% endcode %}
{% endtab %}

{% tab title="Axios" %}
{% code title="example.js" %}
```javascript
const axios = require('axios');

const response = await axios.post('https://shared.eu-central-1.getblock.io/<ACCESS-TOKEN>/chains/main/blocks/head/helpers/scripts/run_operation', {
    operation: {
        branch: 'BMbranch...',
        contents: [
            {
                kind: 'transaction',
                source: 'tz1...',
                fee: '500',
                counter: '12346',
                gas_limit: '1040000',
                storage_limit: '300',
                amount: '1000000',
                destination: 'tz1dest...'
            }
        ],
        signature: 'sig...'
    },
    chain_id: 'NetXdQprcVkpaWU'
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
    'https://shared.eu-central-1.getblock.io/<ACCESS-TOKEN>/chains/main/blocks/head/helpers/scripts/run_operation',
    headers={'Content-Type': 'application/json'},
    json={
        'operation': {
            'branch': 'BMbranch...',
            'contents': [
                {
                    'kind': 'transaction',
                    'source': 'tz1...',
                    'fee': '500',
                    'counter': '12346',
                    'gas_limit': '1040000',
                    'storage_limit': '300',
                    'amount': '1000000',
                    'destination': 'tz1dest...'
                }
            ],
            'signature': 'sig...'
        },
        'chain_id': 'NetXdQprcVkpaWU'
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
        .post("https://shared.eu-central-1.getblock.io/<ACCESS-TOKEN>/chains/main/blocks/head/helpers/scripts/run_operation")
        .header("Content-Type", "application/json")
        .json(&json!({
            "operation": {
                "branch": "BMbranch...",
                "contents": [
                    {
                        "kind": "transaction",
                        "source": "tz1...",
                        "fee": "500",
                        "counter": "12346",
                        "gas_limit": "1040000",
                        "storage_limit": "300",
                        "amount": "1000000",
                        "destination": "tz1dest..."
                    }
                ],
                "signature": "sig..."
            },
            "chain_id": "NetXdQprcVkpaWU"
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
    "contents": [
        {
            "kind": "transaction",
            "metadata": {
                "operation_result": {
                    "status": "applied",
                    "consumed_milligas": "168000"
                }
            }
        }
    ]
}
```

## Response Fields

| Field                    | Type   | Description                                |
| ------------------------ | ------ | ------------------------------------------ |
| contents                 | array  | Operation contents with simulated metadata |
| operation\_result.status | string | applied / failed / backtracked             |
| consumed\_milligas       | string | Gas the operation would consume            |

## Use Cases

* **Dry Run**: Validate an operation and read gas before sending
* **Fee Sizing**: Derive gas and storage limits from a simulation

## Error Handling

| Error                     | Message       | Description                                       |
| ------------------------- | ------------- | ------------------------------------------------- |
| 400 / Bad request         | Bad request   | A path or body parameter was invalid              |
| 404 / Not found           | Not found     | The block, contract, or resource does not exist   |
| 403 / RBAC: access denied | Access denied | The GetBlock access token is missing or incorrect |
