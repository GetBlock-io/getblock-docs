---
description: >-
  Example code for the getBlockCommitment JSON-RPC method. Complete guide on how
  to use getBlockCommitment JSON-RPC in GetBlock Web3 documentation.
---

# getBlockCommitment - Solana

This method returns the stake-weighted commitment recorded for a block, expressed as lamports of stake voting at each lockout depth. It also returns the cluster's total active stake.

## Parameters

| Parameter | Type   | Required | Description                       |
| --------- | ------ | -------- | --------------------------------- |
| slot      | number | Yes      | Slot number of the block to query |

## Request

{% tabs %}
{% tab title="cURL" %}
{% code overflow="wrap" %}
```bash
curl --location --request POST 'https://shared.eu-central-1.getblock.io/<ACCESS-TOKEN>/' \
--header 'Content-Type: application/json' \
--data-raw '{
    "jsonrpc": "2.0",
    "method": "getBlockCommitment",
    "params": [397234561],
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

const commitment = await connection.getBlockCommitment(397234561);

console.log(commitment.totalStake);
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
        'method': 'getBlockCommitment',
        'params': [397234561],
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
            "method": "getBlockCommitment",
            "params": [397234561],
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
        "commitment": [
            0,
            0,
            0,
            0,
            0,
            0,
            0,
            0,
            0,
            0,
            0,
            0,
            0,
            0,
            0,
            0,
            0,
            0,
            0,
            0,
            0,
            0,
            0,
            0,
            0,
            0,
            0,
            0,
            0,
            0,
            0,
            397847362198472
        ],
        "totalStake": 397847362198472
    }
}
```

## Response Parameters

| Parameter | Type   | Description                                                |
| --------- | ------ | ---------------------------------------------------------- |
| jsonrpc   | string | JSON-RPC protocol version ("2.0")                          |
| id        | string | Request identifier matching the request                    |
| result    | object | Object holding the commitment array and total active stake |

### Result Object

| Field      | Type   | Description                                    |
| ---------- | ------ | ---------------------------------------------- |
| commitment | array  | null                                           |
| totalStake | number | Total active stake on the cluster, in lamports |

## Use Cases

* **Finality Verification**: Confirm a block carries supermajority stake before settling value
* **Bridge Security**: Gate cross-chain message relay on a stake-weight threshold
* **Consensus Research**: Study lockout depth distribution across slots
* **Custody Policy**: Require an explicit stake threshold before crediting a deposit

## Error Handling

| Error Code | Message                                                            | Description                                                        |
| ---------- | ------------------------------------------------------------------ | ------------------------------------------------------------------ |
| -32007     | Slot was skipped, or missing due to ledger jump to recent snapshot | The slot was skipped by the leader or predates the node's snapshot |
| -32009     | Slot was skipped, or missing in long-term storage                  | The slot is absent from the node's long-term ledger storage        |
| -32602     | Invalid params                                                     | Slot is negative or not an integer                                 |
| -32603     | Internal error                                                     | Node failed to read the block commitment cache                     |
