---
description: >-
  Example code for the sendTransaction JSON-RPC method. Complete guide on how to
  use the sendTransaction JSON-RPC method in the GetBlock Web3 documentation.
---

# sendTransaction - Solana

This method submits a fully signed transaction to the cluster and returns its first signature. The node forwards the transaction to the current leader without waiting for confirmation.

## Parameters

| Parameter   | Type   | Required | Description                                            |
| ----------- | ------ | -------- | ------------------------------------------------------ |
| transaction | string | Yes      | Fully signed transaction, base58 or base64 encoded     |
| config      | object | No       | Configuration object controlling preflight and retries |

### Config Object

| Field               | Type    | Required | Description                                                              |
| ------------------- | ------- | -------- | ------------------------------------------------------------------------ |
| encoding            | string  | No       | Encoding of the transaction string: base58 or base64. Defaults to base58 |
| skipPreflight       | boolean | No       | Skip the preflight simulation and signature verification checks          |
| preflightCommitment | string  | No       | Commitment used for the preflight simulation. Defaults to finalized      |
| maxRetries          | number  | No       | Maximum times the node rebroadcasts to the leader                        |
| minContextSlot      | number  | No       | Minimum slot at which preflight should be evaluated                      |

## Request

{% tabs %}
{% tab title="cURL" %}
{% code overflow="wrap" %}
```bash
curl --location --request POST 'https://shared.eu-central-1.getblock.io/<ACCESS-TOKEN>/' \
--header 'Content-Type: application/json' \
--data-raw '{
    "jsonrpc": "2.0",
    "method": "sendTransaction",
    "params": ["4hXTCkRzt9WyecNzV1XPgCDfGAZzQKNxLXgynz5QDuWWPSAZBZSHptvWRL3BjCvzUXRdKvHL2b7yGrRQcWyaqsaBCncVG7BFggS8w9snUts67BSh3EqKpXLUm5UMHfD7ZBe9GhARjbNQMLJ1QD3Spr6oMTBU6EhdB4RD8CP2xUxr2u3d6fos36PD98XS6oX8TQjLpsMwncs5DAMiD4nNnR8NBfyghGCWvCVifVwvA8B8TJxE1aiyiv2L429BCWfyzAme5sZW8rDb14NeCQHhZbtNqfXhcp2tAnaAT", {"skipPreflight": false, "preflightCommitment": "confirmed", "maxRetries": 3}],
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

const txSignature = await connection.sendRawTransaction(
  signedTransaction.serialize(),
  { skipPreflight: false, maxRetries: 3 }
);

console.log(txSignature);
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
        'method': 'sendTransaction',
        'params': ["4hXTCkRzt9WyecNzV1XPgCDfGAZzQKNxLXgynz5QDuWWPSAZBZSHptvWRL3BjCvzUXRdKvHL2b7yGrRQcWyaqsaBCncVG7BFggS8w9snUts67BSh3EqKpXLUm5UMHfD7ZBe9GhARjbNQMLJ1QD3Spr6oMTBU6EhdB4RD8CP2xUxr2u3d6fos36PD98XS6oX8TQjLpsMwncs5DAMiD4nNnR8NBfyghGCWvCVifVwvA8B8TJxE1aiyiv2L429BCWfyzAme5sZW8rDb14NeCQHhZbtNqfXhcp2tAnaAT", {"skipPreflight": false, "preflightCommitment": "confirmed", "maxRetries": 3}],
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
            "method": "sendTransaction",
            "params": ["4hXTCkRzt9WyecNzV1XPgCDfGAZzQKNxLXgynz5QDuWWPSAZBZSHptvWRL3BjCvzUXRdKvHL2b7yGrRQcWyaqsaBCncVG7BFggS8w9snUts67BSh3EqKpXLUm5UMHfD7ZBe9GhARjbNQMLJ1QD3Spr6oMTBU6EhdB4RD8CP2xUxr2u3d6fos36PD98XS6oX8TQjLpsMwncs5DAMiD4nNnR8NBfyghGCWvCVifVwvA8B8TJxE1aiyiv2L429BCWfyzAme5sZW8rDb14NeCQHhZbtNqfXhcp2tAnaAT", {"skipPreflight": false, "preflightCommitment": "confirmed", "maxRetries": 3}],
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
    "result": "5wHu1qwD4kLwYqPmy1uDCgpiJ1qGpVYU3n5aHT6bSWUE1JzcbQCPSDLDPGrFyqmLLzQjLNPTPtRZbNbUJQNMkaWq"
}
```
{% endcode %}

## Response Parameters

| Parameter | Type   | Description                                                 |
| --------- | ------ | ----------------------------------------------------------- |
| jsonrpc   | string | JSON-RPC protocol version ("2.0")                           |
| id        | string | Request identifier matching the request                     |
| result    | string | Base58-encoded first signature of the submitted transaction |

## Use Cases

* **Transfer Submission**: Broadcast a signed SOL or SPL Token transfer
* **Latency Tuning**: Set skipPreflight to true once simulation has already passed
* **Retry Control**: Cap node-side rebroadcasts with maxRetries and retry from the client
* **Swap Execution**: Submit a Jupiter route transaction built and signed client-side
* **Program Invocation**: Send an Anchor instruction bundle to a deployed program

## Error Handling

| Error Code | Message                                    | Description                                                                            |
| ---------- | ------------------------------------------ | -------------------------------------------------------------------------------------- |
| -32602     | Invalid params                             | Transaction cannot be deserialized or the encoding does not match                      |
| -32003     | Transaction signature verification failure | One or more signatures are missing or invalid                                          |
| -32002     | Transaction simulation failed              | Preflight simulation reverted, most often BlockhashNotFound or InsufficientFundsForFee |
| -32013     | Transaction signature length mismatch      | Signature count does not match the message header                                      |
| -32005     | Node is unhealthy                          | The node cannot reach the current leader                                               |
