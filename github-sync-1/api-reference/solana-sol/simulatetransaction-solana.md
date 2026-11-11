---
description: >-
  Example code for the simulateTransaction JSON-RPC method. Complete guide on
  how to use the simulateTransaction JSON-RPC method in the GetBlock Web3
  documentation.
---

# simulateTransaction - Solana

This method simulates a transaction against the current bank state without broadcasting it. The response includes program logs, compute units consumed, and any execution error.

## Parameters

| Parameter   | Type   | Required | Description                                           |
| ----------- | ------ | -------- | ----------------------------------------------------- |
| transaction | string | Yes      | Transaction to simulate, base58 or base64 encoded     |
| config      | object | No       | Configuration object controlling simulation behaviour |

### Config Object

| Field                  | Type    | Required | Description                                                                        |
| ---------------------- | ------- | -------- | ---------------------------------------------------------------------------------- |
| encoding               | string  | No       | Encoding of the transaction string: base58 or base64                               |
| commitment             | string  | No       | Commitment level: processed, confirmed, or finalized. Defaults to finalized        |
| sigVerify              | boolean | No       | Verify transaction signatures during simulation                                    |
| replaceRecentBlockhash | boolean | No       | Replace the transaction blockhash with the most recent one                         |
| accounts               | object  | No       | Accounts configuration specifying addresses and encoding to return post-simulation |
| innerInstructions      | boolean | No       | Include inner instructions in the response                                         |
| minContextSlot         | number  | No       | Minimum slot the simulation can be evaluated at                                    |

## Request

{% tabs %}
{% tab title="cURL" %}
{% code overflow="wrap" %}
```bash
curl --location --request POST 'https://shared.eu-central-1.getblock.io/<ACCESS-TOKEN>/' \
--header 'Content-Type: application/json' \
--data-raw '{
    "jsonrpc": "2.0",
    "method": "simulateTransaction",
    "params": ["AQAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAB", {"encoding": "base64", "replaceRecentBlockhash": true, "commitment": "processed"}],
    "id": "getblock.io"
}'
```
{% endcode %}
{% endtab %}

{% tab title="@solana/web3.js" %}
{% code title="example.js" %}
```javascript
const { Connection } = require('@solana/web3.js');

const connection = new Connection('https://shared.eu-central-1.getblock.io/<ACCESS-TOKEN>/', 'processed');

const { value } = await connection.simulateTransaction(transaction, {
  replaceRecentBlockhash: true
});

console.log(value.err, value.unitsConsumed, value.logs);
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
        'method': 'simulateTransaction',
        'params': ["AQAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAB", {"encoding": "base64", "replaceRecentBlockhash": true, "commitment": "processed"}],
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
            "method": "simulateTransaction",
            "params": ["AQAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAB", {"encoding": "base64", "replaceRecentBlockhash": true, "commitment": "processed"}],
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
            "apiVersion": "2.3.6",
            "slot": 397234561
        },
        "value": {
            "accounts": null,
            "err": null,
            "innerInstructions": null,
            "logs": [
                "Program 11111111111111111111111111111111 invoke [1]",
                "Program 11111111111111111111111111111111 success"
            ],
            "replacementBlockhash": {
                "blockhash": "9zJqLXK9pMhQZ4tGqVvUcxrKpKQ3pMSJ1nZmVeXtBqTd",
                "lastValidBlockHeight": 375812409
            },
            "returnData": null,
            "unitsConsumed": 150
        }
    }
}
```

## Response Parameters

| Parameter | Type   | Description                                                  |
| --------- | ------ | ------------------------------------------------------------ |
| jsonrpc   | string | JSON-RPC protocol version ("2.0")                            |
| id        | string | Request identifier matching the request                      |
| result    | object | RPC response object where value holds the simulation outcome |

### Value Object

| Field             | Type   | Description                                         |
| ----------------- | ------ | --------------------------------------------------- |
| err               | object | null                                                |
| logs              | array  | null                                                |
| accounts          | array  | null                                                |
| unitsConsumed     | number | Compute units consumed by the simulated transaction |
| returnData        | object | null                                                |
| innerInstructions | array  | null                                                |

## Use Cases

* **Pre-Send Validation**: Catch InstructionError conditions before paying a fee
* **Compute Unit Sizing**: Read unitsConsumed to set an accurate compute unit limit
* **Log Debugging**: Inspect program logs during Anchor program development
* **Slippage Checks**: Simulate a swap route to confirm the output amount before signing
* **Account Diffing**: Return post-simulation account state to preview balance changes

## Error Handling

| Error Code | Message                                    | Description                                                          |
| ---------- | ------------------------------------------ | -------------------------------------------------------------------- |
| -32602     | Invalid params                             | Transaction cannot be deserialized or accounts config exceeds limits |
| -32003     | Transaction signature verification failure | sigVerify was enabled and a signature is invalid                     |
| -32002     | Transaction simulation failed              | Execution reverted, with the program error in the value.err field    |
| -32016     | Minimum context slot has not been reached  | The node has not yet processed minContextSlot                        |
