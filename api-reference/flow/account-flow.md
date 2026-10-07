---
description: >-
  Example code for the /v1/accounts/{address} REST method. Complete guide on how
  to use the /v1/accounts/{address} REST method in GetBlock Web3 documentation
---

# account - Flow

Returns a Flow account: its FLOW balance, public keys, and deployed Cadence contracts. By default it reads at the sealed block; use `block_height` to read historical state and `expand` to include keys and contracts.

## Endpoint

```http
GET /v1/accounts/{address}
```

## Path Parameters

| Parameter | Type   | Description                                                  |
| --------- | ------ | ------------------------------------------------------------ |
| address   | string | Flow account address (16 hex chars, e.g. 0x1654653399040a61) |

## Query Parameters

| Parameter     | Type   | Description                                      |
| ------------- | ------ | ------------------------------------------------ |
| block\_height | string | Block height, or final / sealed (default sealed) |
| expand        | string | Comma-separated: keys, contracts                 |

## Example

{% tabs %}
{% tab title="cURL" %}
{% code overflow="wrap" %}
```bash
export FLOW_REST=https://shared.eu-central-1.getblock.io/<ACCESS-TOKEN>/

curl "${FLOW_REST}v1/accounts/0x1654653399040a61?expand=keys,contracts"
```
{% endcode %}
{% endtab %}

{% tab title="Axios" %}
{% code title="example.js" %}
```javascript
const axios = require('axios');

const response = await axios.get('https://shared.eu-central-1.getblock.io/<ACCESS-TOKEN>/v1/accounts/0x1654653399040a61', {
    params: { expand: 'keys,contracts' },
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
    'https://shared.eu-central-1.getblock.io/<ACCESS-TOKEN>/v1/accounts/0x1654653399040a61',
    headers={'Content-Type': 'application/json'},
    params={'expand': 'keys,contracts'}
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
        .get("https://shared.eu-central-1.getblock.io/<ACCESS-TOKEN>/v1/accounts/0x1654653399040a61")
        .query(&[("expand", "keys,contracts")])
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
    "address": "1654653399040a61",
    "balance": "150000000",
    "keys": [
        {
            "index": "0",
            "public_key": "0xabc...",
            "signing_algorithm": "ECDSA_P256",
            "hashing_algorithm": "SHA3_256",
            "weight": "1000",
            "sequence_number": "42",
            "revoked": false
        }
    ],
    "contracts": {
        "FlowToken": "aW1wb3J0..."
    }
}
```

## Response Fields

| Field     | Type   | Description                                           |
| --------- | ------ | ----------------------------------------------------- |
| address   | string | Account address without 0x prefix                     |
| balance   | string | FLOW balance in the smallest unit (1 FLOW = 1e8)      |
| keys      | array  | Account public keys with weights and sequence numbers |
| contracts | object | Map of contract name to base64 Cadence code           |

## Use Cases

* **Account State**: Read balance, keys, and contracts in one call
* **Key Management**: Inspect weights and sequence numbers before signing

## Error Handling

| Error                     | Message       | Description                                                 |
| ------------------------- | ------------- | ----------------------------------------------------------- |
| 400 / Bad request         | Bad request   | A path, query, or body parameter was invalid                |
| 404 / Not found           | Not found     | The block, account, transaction, or resource does not exist |
| 403 / RBAC: access denied | Access denied | The GetBlock access token is missing or incorrect           |
