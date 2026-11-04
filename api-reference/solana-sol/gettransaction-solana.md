---
description: >-
  Example code for the getTransaction JSON-RPC method. Complete guide on how to
  use the getTransaction JSON-RPC method in the GetBlock Web3 documentation.
---

# getTransaction - Solana

This method returns a confirmed transaction by signature, including its instructions, account keys, logs, and balance changes. Versioned transactions require `maxSupportedTransactionVersion` to be set.

## Parameters

| Parameter | Type   | Required | Description                                                   |
| --------- | ------ | -------- | ------------------------------------------------------------- |
| signature | string | Yes      | Base58-encoded transaction signature                          |
| config    | object | No       | Configuration object controlling encoding and version support |

### Config Object

| Field                          | Type   | Required | Description                                                                            |
| ------------------------------ | ------ | -------- | -------------------------------------------------------------------------------------- |
| encoding                       | string | No       | Transaction encoding: json, jsonParsed, base58, or base64                              |
| commitment                     | string | No       | Commitment level: processed, confirmed, or finalized. Defaults to finalized            |
| maxSupportedTransactionVersion | number | No       | Highest transaction version the client can decode. Set to 0 for versioned transactions |

## Request

{% tabs %}
{% tab title="cURL" %}
{% code overflow="wrap" %}
```bash
curl --location --request POST 'https://shared.eu-central-1.getblock.io/<ACCESS-TOKEN>/' \
--header 'Content-Type: application/json' \
--data-raw '{
    "jsonrpc": "2.0",
    "method": "getTransaction",
    "params": ["5wHu1qwD4kLwYqPmy1uDCgpiJ1qGpVYU3n5aHT6bSWUE1JzcbQCPSDLDPGrFyqmLLzQjLNPTPtRZbNbUJQNMkaWq", {"encoding": "jsonParsed", "maxSupportedTransactionVersion": 0}],
    "id": "getblock.io"
}'
```
{% endcode %}
{% endtab %}

{% tab title="@solana/web3.js" %}
{% code title="example.js" %}
```javascript
const { Connection } = require('@solana/web3.js');

const connection = new Connection('https://shared.eu-central-1.getblock.io/<ACCESS-TOKEN>/', 'confirmed');

const transaction = await connection.getParsedTransaction(
  '5wHu1qwD4kLwYqPmy1uDCgpiJ1qGpVYU3n5aHT6bSWUE1JzcbQCPSDLDPGrFyqmLLzQjLNPTPtRZbNbUJQNMkaWq',
  { maxSupportedTransactionVersion: 0 }
);

console.log(transaction?.meta?.fee);
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
        'method': 'getTransaction',
        'params': ["5wHu1qwD4kLwYqPmy1uDCgpiJ1qGpVYU3n5aHT6bSWUE1JzcbQCPSDLDPGrFyqmLLzQjLNPTPtRZbNbUJQNMkaWq", {"encoding": "jsonParsed", "maxSupportedTransactionVersion": 0}],
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
            "method": "getTransaction",
            "params": ["5wHu1qwD4kLwYqPmy1uDCgpiJ1qGpVYU3n5aHT6bSWUE1JzcbQCPSDLDPGrFyqmLLzQjLNPTPtRZbNbUJQNMkaWq", {"encoding": "jsonParsed", "maxSupportedTransactionVersion": 0}],
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
    "result": {
        "blockTime": 1774267845,
        "slot": 397234561,
        "meta": {
            "computeUnitsConsumed": 84213,
            "err": null,
            "fee": 5000,
            "innerInstructions": [],
            "logMessages": [
                "Program JUP6LkbZbjS1jKKwapdHNy74zcZ3tLUZoi5QNyVTaV4 invoke [1]"
            ],
            "postBalances": [
                12043900,
                2039280
            ],
            "preBalances": [
                12048900,
                2039280
            ],
            "status": {
                "Ok": null
            }
        },
        "transaction": {
            "message": {
                "accountKeys": [],
                "recentBlockhash": "9zJqLXK9pMhQZ4tGqVvUcxrKpKQ3pMSJ1nZmVeXtBqTd"
            },
            "signatures": [
                "5wHu1qwD4kLwYqPmy1uDCgpiJ1qGpVYU3n5aHT6bSWUE1JzcbQCPSDLDPGrFyqmLLzQjLNPTPtRZbNbUJQNMkaWq"
            ]
        },
        "version": 0
    }
}
```
{% endcode %}

## Response Parameters

| Parameter | Type   | Description                             |
| --------- | ------ | --------------------------------------- |
| jsonrpc   | string | JSON-RPC protocol version ("2.0")       |
| id        | string | Request identifier matching the request |
| result    | object | null                                    |

### Result Object

| Field       | Type   | Description                                                         |
| ----------- | ------ | ------------------------------------------------------------------- |
| slot        | number | Slot containing the transaction                                     |
| blockTime   | number | null                                                                |
| transaction | object | Transaction message and signatures in the requested encoding        |
| meta        | object | Execution metadata including fee, balances, logs, and compute units |
| version     | number | string                                                              |

## Use Cases

* **Receipt Rendering**: Display instruction-level detail for a completed transfer
* **Log Inspection**: Read `logMessages` to debug a failing program invocation
* **Balance Deltas**: Compare `preBalances` and `postBalances` to compute net movement
* **Compute Budgeting**: Read `computeUnitsConsumed` to right-size a compute unit limit
* **Token Transfer Parsing**: Use `jsonParsed` encoding to extract SPL Token transfer amounts

## Error Handling

| Error Code | Message                                                           | Description                                                              |
| ---------- | ----------------------------------------------------------------- | ------------------------------------------------------------------------ |
| -32602     | Invalid params                                                    | Signature is malformed or an unsupported encoding was requested          |
| -32015     | Transaction version (0) is not supported by the requesting client | `maxSupportedTransactionVersion` was omitted for a versioned transaction |
| -32011     | Transaction history is not available from this node               | The node does not retain the ledger range containing the signature       |
| -32603     | Internal error                                                    | Node failed to read the transaction from blockstore                      |
