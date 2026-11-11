---
description: >-
  Example code for the /chains/main/blocks/{block}/helpers/forge/operations REST
  method. Complete guide on how to use the /helpers/forge/operations REST method
  in GetBlock Web3 documentation
---

# forge-operations - Tezos

Serializes (forges) an operation's JSON representation into the binary hex payload that must be signed before injection.

{% hint style="warning" %}
For production, forge locally rather than trusting a remote node.
{% endhint %}

## Endpoint

```http
POST /chains/main/blocks/{block}/helpers/forge/operations
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

curl -X POST "${TEZOS_REST}chains/main/blocks/head/helpers/forge/operations" \
  -H "Content-Type: application/json" \
  -d '{"branch": "BMbranch...", "contents": [{"kind": "transaction", "source": "tz1...", "fee": "500", "counter": "12346", "gas_limit": "1040000", "storage_limit": "300", "amount": "1000000", "destination": "tz1dest..."}]}'
```
{% endcode %}
{% endtab %}

{% tab title="Axios" %}
{% code title="example.js" %}
```javascript
const axios = require('axios');

const response = await axios.post('https://shared.eu-central-1.getblock.io/<ACCESS-TOKEN>/chains/main/blocks/head/helpers/forge/operations', {
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
    ]
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
    'https://shared.eu-central-1.getblock.io/<ACCESS-TOKEN>/chains/main/blocks/head/helpers/forge/operations',
    headers={'Content-Type': 'application/json'},
    json={
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
        ]
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
        .post("https://shared.eu-central-1.getblock.io/<ACCESS-TOKEN>/chains/main/blocks/head/helpers/forge/operations")
        .header("Content-Type", "application/json")
        .json(&json!({
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
            ]
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
"ce69c5713db...forgedhex..."
```

## Response Fields

| Field  | Type   | Description                                     |
| ------ | ------ | ----------------------------------------------- |
| (root) | string | Hex-encoded forged operation bytes to be signed |

## Use Cases

* **Transaction Building**: Produce signable bytes from operation JSON
* **Offline Signing**: Pair with a local signer

## Error Handling

| Error                     | Message       | Description                                       |
| ------------------------- | ------------- | ------------------------------------------------- |
| 400 / Bad request         | Bad request   | A path or body parameter was invalid              |
| 404 / Not found           | Not found     | The block, contract, or resource does not exist   |
| 403 / RBAC: access denied | Access denied | The GetBlock access token is missing or incorrect |
