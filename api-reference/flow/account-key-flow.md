---
description: >-
  Example code for the /v1/accounts/{address}/keys/{index} REST method. Complete
  guide on how to use the /v1/accounts/{address}/keys/{index} REST method in
  GetBlock Web3 documentation
---

# account-key - Flow

Returns a single public key of an account by its key index, including its signing and hashing algorithms, weight, sequence number, and revocation status.

## Endpoint

```http
GET /v1/accounts/{address}/keys/{index}
```

## Path Parameters

| Parameter | Type   | Description                         |
| --------- | ------ | ----------------------------------- |
| address   | string | Flow account address (16 hex chars) |
| index     | string | Key index (integer, starting at 0)  |

## Query Parameters

| Parameter     | Type   | Description                     |
| ------------- | ------ | ------------------------------- |
| block\_height | string | Block height, or final / sealed |

## Example

{% tabs %}
{% tab title="cURL" %}
{% code overflow="wrap" %}
```bash
export FLOW_REST=https://shared.eu-central-1.getblock.io/<ACCESS-TOKEN>/

curl "${FLOW_REST}v1/accounts/0x1654653399040a61/keys/0"
```
{% endcode %}
{% endtab %}

{% tab title="Axios" %}
{% code title="example.js" %}
```javascript
const axios = require('axios');

const response = await axios.get('https://shared.eu-central-1.getblock.io/<ACCESS-TOKEN>/v1/accounts/0x1654653399040a61/keys/0', {
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
    'https://shared.eu-central-1.getblock.io/<ACCESS-TOKEN>/v1/accounts/0x1654653399040a61/keys/0',
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
        .get("https://shared.eu-central-1.getblock.io/<ACCESS-TOKEN>/v1/accounts/0x1654653399040a61/keys/0")
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
    "index": "0",
    "public_key": "0xabc...",
    "signing_algorithm": "ECDSA_P256",
    "hashing_algorithm": "SHA3_256",
    "sequence_number": "42",
    "weight": "1000",
    "revoked": false
}
```

## Response Fields

| Field              | Type    | Description                                |
| ------------------ | ------- | ------------------------------------------ |
| index              | string  | Key index                                  |
| signing\_algorithm | string  | ECDSA\_P256 or ECDSA\_secp256k1            |
| weight             | string  | Key weight (1000 = full signing authority) |
| sequence\_number   | string  | Next sequence number for this key          |
| revoked            | boolean | Whether the key is revoked                 |

## Use Cases

* **Signing Setup**: Read the sequence number before building a transaction
* **Security Audit**: Check key weights and revocation

## Error Handling

| Error                     | Message       | Description                                                 |
| ------------------------- | ------------- | ----------------------------------------------------------- |
| 400 / Bad request         | Bad request   | A path, query, or body parameter was invalid                |
| 404 / Not found           | Not found     | The block, account, transaction, or resource does not exist |
| 403 / RBAC: access denied | Access denied | The GetBlock access token is missing or incorrect           |
