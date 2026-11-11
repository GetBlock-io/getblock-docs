---
description: >-
  Example code for the getSignaturesForAddress JSON-RPC method. Complete guide
  on how to use the getSignaturesForAddress JSON-RPC method in the GetBlock Web3
  documentation.
---

# getSignaturesForAddress - Solana

This method returns confirmed transaction signatures that reference a given address, ordered from newest to oldest. Results are paginated with the `before` and `until` cursors.

## Parameters

| Parameter | Type   | Required | Description                                                |
| --------- | ------ | -------- | ---------------------------------------------------------- |
| address   | string | Yes      | Base58-encoded account Pubkey to search for                |
| config    | object | No       | Configuration object controlling pagination and commitment |

### Config Object

| Field          | Type   | Required | Description                                                                 |
| -------------- | ------ | -------- | --------------------------------------------------------------------------- |
| limit          | number | No       | Maximum signatures to return, between 1 and 1000. Defaults to 1000          |
| before         | string | No       | Start searching backwards from this signature, exclusive                    |
| until          | string | No       | Search until this signature is reached, exclusive                           |
| commitment     | string | No       | Commitment level: processed, confirmed, or finalized. Defaults to finalized |
| minContextSlot | number | No       | Minimum slot the request can be evaluated at                                |

## Request

{% tabs %}
{% tab title="cURL" %}
{% code overflow="wrap" %}
```bash
curl --location --request POST 'https://shared.eu-central-1.getblock.io/<ACCESS-TOKEN>/' \
--header 'Content-Type: application/json' \
--data-raw '{
    "jsonrpc": "2.0",
    "method": "getSignaturesForAddress",
    "params": ["JUP6LkbZbjS1jKKwapdHNy74zcZ3tLUZoi5QNyVTaV4", {"limit": 3}],
    "id": "getblock.io"
}'
```
{% endcode %}
{% endtab %}

{% tab title="@solana/web3.js" %}
{% code title="example.js" %}
```javascript
const { Connection, PublicKey } = require('@solana/web3.js');

const connection = new Connection('https://shared.eu-central-1.getblock.io/<ACCESS-TOKEN>/', 'confirmed');

const signatures = await connection.getSignaturesForAddress(
  new PublicKey('JUP6LkbZbjS1jKKwapdHNy74zcZ3tLUZoi5QNyVTaV4'),
  { limit: 3 }
);

console.log(signatures.map((s) => s.signature));
```
{% endcode %}
{% endtab %}

{% tab title="Request" %}
{% code title="example.py" %}
```python
import requests

response = requests.post(
    'https://shared.eu-central-1.getblock.io/<ACCESS-TOKEN>/',
    headers={'Content-Type': 'application/json'},
    json={
        'jsonrpc': '2.0',
        'method': 'getSignaturesForAddress',
        'params': ["JUP6LkbZbjS1jKKwapdHNy74zcZ3tLUZoi5QNyVTaV4", {"limit": 3}],
        'id': 'getblock.io'
    }
)

print(response.json()['result'])
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
        .post("https://shared.eu-central-1.getblock.io/<ACCESS-TOKEN>/")
        .header("Content-Type", "application/json")
        .json(&json!({
            "jsonrpc": "2.0",
            "method": "getSignaturesForAddress",
            "params": ["JUP6LkbZbjS1jKKwapdHNy74zcZ3tLUZoi5QNyVTaV4", {"limit": 3}],
            "id": "getblock.io"
        }))
        .send()
        .await?
        .json::<Value>()
        .await?;

    println!("Result: {}", response["result"]);
    Ok(())
}
```
{% endcode %}
{% endtab %}
{% endtabs %}

## Response

{% code overflow="wrap" %}
```json
{
    "jsonrpc": "2.0",
    "id": "getblock.io",
    "result": [
        {
            "blockTime": 1774267845,
            "confirmationStatus": "finalized",
            "err": null,
            "memo": null,
            "signature": "5wHu1qwD4kLwYqPmy1uDCgpiJ1qGpVYU3n5aHT6bSWUE1JzcbQCPSDLDPGrFyqmLLzQjLNPTPtRZbNbUJQNMkaWq",
            "slot": 397234561
        }
    ]
}
```
{% endcode %}

## Response Parameters

| Parameter | Type   | Description                                          |
| --------- | ------ | ---------------------------------------------------- |
| jsonrpc   | string | JSON-RPC protocol version ("2.0")                    |
| id        | string | Request identifier matching the request              |
| result    | array  | Array of transaction signature objects, newest first |

### Signature Entry

| Field              | Type   | Description                                             |
| ------------------ | ------ | ------------------------------------------------------- |
| signature          | string | Base58-encoded transaction signature                    |
| slot               | number | Slot containing the transaction                         |
| err                | object | null                                                    |
| memo               | string | null                                                    |
| blockTime          | number | null                                                    |
| confirmationStatus | string | Confirmation status: processed, confirmed, or finalized |

## Use Cases

* **Wallet History**: Build a paginated activity feed for a user address
* **Program Monitoring**: Track recent interactions with a deployed program
* **Backfilling**: Walk history with the `before` cursor to load an address archive
* **Failure Analysis**: Filter for entries where `err` is non-null to find reverted attempts
* **Memo Indexing**: Extract memo payloads for payment reconciliation

## Error Handling

| Error Code | Message                                             | Description                                                       |
| ---------- | --------------------------------------------------- | ----------------------------------------------------------------- |
| -32602     | Invalid params                                      | Limit outside 1 to 1000, or a malformed before or until signature |
| -32011     | Transaction history is not available from this node | The node runs without a full transaction index                    |
| -32603     | Internal error                                      | Node failed to read the address signature index                   |
