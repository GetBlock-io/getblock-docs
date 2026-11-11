---
description: >-
  Example code for the /v1/transactions/{id} REST method. Complete guide on how
  to use the /v1/transactions/{id} REST method in GetBlock Web3 documentation
---

# transaction - Flow

Returns a transaction by ID: its Cadence script, arguments, reference block, gas limit, proposer/payer/authorizers, and signatures. Expand the result to include execution status and events.

## Endpoint

```http
GET /v1/transactions/{id}
```

## Path Parameters

| Parameter | Type   | Description          |
| --------- | ------ | -------------------- |
| id        | string | Transaction ID (hex) |

## Query Parameters

| Parameter | Type   | Description   |
| --------- | ------ | ------------- |
| expand    | string | Expand result |

## Example

{% tabs %}
{% tab title="cURL" %}
{% code overflow="wrap" %}
```bash
export FLOW_REST=https://shared.eu-central-1.getblock.io/<ACCESS-TOKEN>/

curl "${FLOW_REST}v1/transactions/a1b2c3d4e5f60718293a4b5c6d7e8f90a1b2c3d4e5f60718293a4b5c6d7e8f90"
```
{% endcode %}
{% endtab %}

{% tab title="Axios" %}
{% code title="example.js" %}
```javascript
const axios = require('axios');

const response = await axios.get('https://shared.eu-central-1.getblock.io/<ACCESS-TOKEN>/v1/transactions/a1b2c3d4e5f60718293a4b5c6d7e8f90a1b2c3d4e5f60718293a4b5c6d7e8f90', {
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
    'https://shared.eu-central-1.getblock.io/<ACCESS-TOKEN>/v1/transactions/a1b2c3d4e5f60718293a4b5c6d7e8f90a1b2c3d4e5f60718293a4b5c6d7e8f90',
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
        .get("https://shared.eu-central-1.getblock.io/<ACCESS-TOKEN>/v1/transactions/a1b2c3d4e5f60718293a4b5c6d7e8f90a1b2c3d4e5f60718293a4b5c6d7e8f90")
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
    "id": "a1b2...",
    "script": "aW1wb3J0IEZsb3dUb2tlbg==",
    "arguments": [
        "eyJ0eXBlIjoi..."
    ],
    "reference_block_id": "3b5f...",
    "gas_limit": "9999",
    "payer": "1654653399040a61",
    "proposal_key": {
        "address": "1654653399040a61",
        "key_index": "0",
        "sequence_number": "42"
    },
    "authorizers": [
        "1654653399040a61"
    ],
    "envelope_signatures": [
        {
            "address": "1654653399040a61",
            "key_index": "0",
            "signature": "MEUC..."
        }
    ]
}
```

## Response Fields

| Field         | Type   | Description                                      |
| ------------- | ------ | ------------------------------------------------ |
| script        | string | Base64-encoded Cadence transaction script        |
| arguments     | array  | Base64-encoded JSON-Cadence arguments            |
| proposal\_key | object | Proposer address, key index, and sequence number |
| payer         | string | Account paying the fee                           |
| authorizers   | array  | Accounts authorizing the transaction             |

## Use Cases

* **Transaction Inspection**: Decode a submitted transaction
* **Audit**: Verify signers and the Cadence payload

## Error Handling

| Error                     | Message       | Description                                                 |
| ------------------------- | ------------- | ----------------------------------------------------------- |
| 400 / Bad request         | Bad request   | A path, query, or body parameter was invalid                |
| 404 / Not found           | Not found     | The block, account, transaction, or resource does not exist |
| 403 / RBAC: access denied | Access denied | The GetBlock access token is missing or incorrect           |
