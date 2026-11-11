---
description: >-
  Example code for the /v1/transactions REST method. Complete guide on how to
  use the /v1/transactions REST method in GetBlock Web3 documentation
---

# send-transaction - Flow

Submits a signed transaction to the network and returns the created transaction, including its ID. The body carries the base64 Cadence script, arguments, reference block, gas limit, proposal key, payer, authorizers, and signatures.

## Endpoint

```http
POST /v1/transactions
```

## Example

{% tabs %}
{% tab title="cURL" %}
{% code overflow="wrap" %}
```bash
export FLOW_REST=https://shared.eu-central-1.getblock.io/<ACCESS-TOKEN>/

curl -X POST "${FLOW_REST}v1/transactions" \
  -H "Content-Type: application/json" \
  -d '{"script": "aW1wb3J0IEZsb3dUb2tlbg==", "arguments": ["eyJ0eXBlIjoiVUZpeDY0In0="], "reference_block_id": "3b5f...", "gas_limit": "9999", "payer": "1654653399040a61", "proposal_key": {"address": "1654653399040a61", "key_index": "0", "sequence_number": "42"}, "authorizers": ["1654653399040a61"], "payload_signatures": [], "envelope_signatures": [{"address": "1654653399040a61", "key_index": "0", "signature": "MEUC..."}]}'
```
{% endcode %}
{% endtab %}

{% tab title="Axios" %}
{% code title="example.js" %}
```javascript
const axios = require('axios');

const response = await axios.post('https://shared.eu-central-1.getblock.io/<ACCESS-TOKEN>/v1/transactions', {
    script: 'aW1wb3J0IEZsb3dUb2tlbg==',
    arguments: ['eyJ0eXBlIjoiVUZpeDY0In0='],
    reference_block_id: '3b5f...',
    gas_limit: '9999',
    payer: '1654653399040a61',
    proposal_key: {
        address: '1654653399040a61',
        key_index: '0',
        sequence_number: '42'
    },
    authorizers: ['1654653399040a61'],
    payload_signatures: [],
    envelope_signatures: [
        { address: '1654653399040a61', key_index: '0', signature: 'MEUC...' }
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
    'https://shared.eu-central-1.getblock.io/<ACCESS-TOKEN>/v1/transactions',
    headers={'Content-Type': 'application/json'},
    json={
        'script': 'aW1wb3J0IEZsb3dUb2tlbg==',
        'arguments': ['eyJ0eXBlIjoiVUZpeDY0In0='],
        'reference_block_id': '3b5f...',
        'gas_limit': '9999',
        'payer': '1654653399040a61',
        'proposal_key': {
            'address': '1654653399040a61',
            'key_index': '0',
            'sequence_number': '42'
        },
        'authorizers': ['1654653399040a61'],
        'payload_signatures': [],
        'envelope_signatures': [
            {'address': '1654653399040a61', 'key_index': '0', 'signature': 'MEUC...'}
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
        .post("https://shared.eu-central-1.getblock.io/<ACCESS-TOKEN>/v1/transactions")
        .header("Content-Type", "application/json")
        .json(&json!({
            "script": "aW1wb3J0IEZsb3dUb2tlbg==",
            "arguments": ["eyJ0eXBlIjoiVUZpeDY0In0="],
            "reference_block_id": "3b5f...",
            "gas_limit": "9999",
            "payer": "1654653399040a61",
            "proposal_key": {
                "address": "1654653399040a61",
                "key_index": "0",
                "sequence_number": "42"
            },
            "authorizers": ["1654653399040a61"],
            "payload_signatures": [],
            "envelope_signatures": [
                { "address": "1654653399040a61", "key_index": "0", "signature": "MEUC..." }
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
{
    "id": "a1b2...",
    "_links": {
        "_self": "/v1/transactions/a1b2..."
    }
}
```

## Response Fields

| Field | Type   | Description                                 |
| ----- | ------ | ------------------------------------------- |
| id    | string | Transaction ID of the submitted transaction |

## Use Cases

* **Transaction Submission**: Broadcast a signed Cadence transaction
* **dApp Writes**: Move FLOW or call contract functions

## Error Handling

| Error                     | Message       | Description                                                 |
| ------------------------- | ------------- | ----------------------------------------------------------- |
| 400 / Bad request         | Bad request   | A path, query, or body parameter was invalid                |
| 404 / Not found           | Not found     | The block, account, transaction, or resource does not exist |
| 403 / RBAC: access denied | Access denied | The GetBlock access token is missing or incorrect           |
