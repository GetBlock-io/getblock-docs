---
description: >-
  Example code for the getSignatureStatuses JSON-RPC method. Complete guide on
  how to use the getSignatureStatuses JSON-RPC method in the GetBlock Web3
  documentation.
---

# getSignatureStatuses - Solana

This method returns the processing status of up to 256 transaction signatures. Signatures outside the node's recent status cache return null unless searchTransactionHistory is enabled.

## Parameters

| Parameter  | Type   | Required | Description                                              |
| ---------- | ------ | -------- | -------------------------------------------------------- |
| signatures | array  | Yes      | Array of up to 256 base58-encoded transaction signatures |
| config     | object | No       | Configuration object controlling history search          |

### Config Object

| Field                    | Type    | Required | Description                                                             |
| ------------------------ | ------- | -------- | ----------------------------------------------------------------------- |
| searchTransactionHistory | boolean | No       | Search the node's ledger for signatures outside the recent status cache |

## Request

{% tabs %}
{% tab title="cURL" %}
{% code overflow="wrap" %}
```bash
curl --location --request POST 'https://shared.eu-central-1.getblock.io/<ACCESS-TOKEN>/' \
--header 'Content-Type: application/json' \
--data-raw '{
    "jsonrpc": "2.0",
    "method": "getSignatureStatuses",
    "params": [["5wHu1qwD4kLwYqPmy1uDCgpiJ1qGpVYU3n5aHT6bSWUE1JzcbQCPSDLDPGrFyqmLLzQjLNPTPtRZbNbUJQNMkaWq"], {"searchTransactionHistory": true}],
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

const { value } = await connection.getSignatureStatuses(
  ['5wHu1qwD4kLwYqPmy1uDCgpiJ1qGpVYU3n5aHT6bSWUE1JzcbQCPSDLDPGrFyqmLLzQjLNPTPtRZbNbUJQNMkaWq'],
  { searchTransactionHistory: true }
);

console.log(value[0]);
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
        'method': 'getSignatureStatuses',
        'params': [["5wHu1qwD4kLwYqPmy1uDCgpiJ1qGpVYU3n5aHT6bSWUE1JzcbQCPSDLDPGrFyqmLLzQjLNPTPtRZbNbUJQNMkaWq"], {"searchTransactionHistory": true}],
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
            "method": "getSignatureStatuses",
            "params": [["5wHu1qwD4kLwYqPmy1uDCgpiJ1qGpVYU3n5aHT6bSWUE1JzcbQCPSDLDPGrFyqmLLzQjLNPTPtRZbNbUJQNMkaWq"], {"searchTransactionHistory": true}],
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

```json
{
    "jsonrpc": "2.0",
    "id": "getblock.io",
    "result": {
        "context": {
            "slot": 397234561
        },
        "value": [
            {
                "confirmationStatus": "finalized",
                "confirmations": null,
                "err": null,
                "slot": 397234561,
                "status": {
                    "Ok": null
                }
            }
        ]
    }
}
```

## Response Parameters

| Parameter | Type   | Description                                                                   |
| --------- | ------ | ----------------------------------------------------------------------------- |
| jsonrpc   | string | JSON-RPC protocol version ("2.0")                                             |
| id        | string | Request identifier matching the request                                       |
| result    | object | RPC response object where value is an array of status objects or null entries |

### Status Entry

| Field              | Type   | Description                                             |
| ------------------ | ------ | ------------------------------------------------------- |
| slot               | number | Slot the transaction was processed in                   |
| confirmations      | number | null                                                    |
| err                | object | null                                                    |
| confirmationStatus | string | Confirmation status: processed, confirmed, or finalized |

## Use Cases

* **Send Confirmation**: Poll a signature after sendTransaction until it reaches finalized
* **Batch Settlement**: Check up to 256 payout signatures in a single request
* **Retry Decisions**: Distinguish dropped transactions from failed ones before rebroadcasting
* **Error Surfacing**: Read the err object to report an InstructionError back to a user

## Error Handling

| Error Code | Message                                             | Description                                                         |
| ---------- | --------------------------------------------------- | ------------------------------------------------------------------- |
| -32602     | Invalid params                                      | More than 256 signatures supplied, or a signature of invalid length |
| -32011     | Transaction history is not available from this node | searchTransactionHistory requested on a node without a full index   |
| -32603     | Internal error                                      | Node failed to read the signature status cache                      |
