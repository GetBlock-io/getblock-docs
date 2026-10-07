---
description: >-
  Example code for the /chains/main/blocks/{block}/helpers/preapply/operations
  REST method. Complete guide on how to use the /helpers/preapply/operations
  REST method in GetBlock Web3 documentation
---

# preapply-operations - Tezos

Pre-applies a signed operation to predict its result and metadata before injection, surfacing errors and balance updates. A final check after signing and before broadcasting.

## Endpoint

```http
POST /chains/main/blocks/{block}/helpers/preapply/operations
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

curl -X POST "${TEZOS_REST}chains/main/blocks/head/helpers/preapply/operations" \
  -H "Content-Type: application/json" \
  -d '[{"protocol": "PsParisC...", "branch": "BMbranch...", "contents": [{"kind": "transaction", "source": "tz1...", "fee": "500", "counter": "12346", "gas_limit": "1040000", "storage_limit": "300", "amount": "1000000", "destination": "tz1dest..."}], "signature": "sig..."}]'
```
{% endcode %}
{% endtab %}

{% tab title="Axios" %}
{% code title="example.js" %}
```javascript
const axios = require('axios');

const response = await axios.post('https://shared.eu-central-1.getblock.io/<ACCESS-TOKEN>/chains/main/blocks/head/helpers/preapply/operations', [
    {
        protocol: 'PsParisC...',
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
    }
], {
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
    'https://shared.eu-central-1.getblock.io/<ACCESS-TOKEN>/chains/main/blocks/head/helpers/preapply/operations',
    headers={'Content-Type': 'application/json'},
    json=[
        {
            'protocol': 'PsParisC...',
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
        }
    ]
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
        .post("https://shared.eu-central-1.getblock.io/<ACCESS-TOKEN>/chains/main/blocks/head/helpers/preapply/operations")
        .header("Content-Type", "application/json")
        .json(&json!([
            {
                "protocol": "PsParisC...",
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
            }
        ]))
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
  {
    "contents": [
      {
        "kind": "transaction",
        "metadata": {
          "operation_result": {
            "status": "applied"
          }
        }
      }
    ]
  }
]
```

## Response Fields

| Field                    | Type   | Description                                |
| ------------------------ | ------ | ------------------------------------------ |
| contents                 | array  | Operation contents with predicted metadata |
| operation\_result.status | string | Predicted status                           |

## Use Cases

* **Pre-flight Check**: Confirm an operation will apply before injecting
* **Error Surfacing**: Catch failures with a signed payload

## Error Handling

| Error                     | Message       | Description                                       |
| ------------------------- | ------------- | ------------------------------------------------- |
| 400 / Bad request         | Bad request   | A path or body parameter was invalid              |
| 404 / Not found           | Not found     | The block, contract, or resource does not exist   |
| 403 / RBAC: access denied | Access denied | The GetBlock access token is missing or incorrect |
