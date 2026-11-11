---
description: >-
  Example code for the getFeeForMessage JSON-RPC method. Complete guide on how
  to use the getFeeForMessage JSON-RPC method in the GetBlock Web3
  documentation.
---

# getFeeForMessage - Solana

This method returns the fee the cluster would charge to process a base64-encoded transaction message. Fees are quoted against a specific blockhash and expire with it.

## Parameters

| Parameter | Type   | Required | Description                                 |
| --------- | ------ | -------- | ------------------------------------------- |
| message   | string | Yes      | Base64-encoded transaction message          |
| config    | object | No       | Configuration object controlling commitment |

### Config Object

| Field          | Type   | Required | Description                                                                 |
| -------------- | ------ | -------- | --------------------------------------------------------------------------- |
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
    "method": "getFeeForMessage",
    "params": ["AQABAgIAAAAMAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAQAA", {"commitment": "processed"}],
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

const { value: fee } = await connection.getFeeForMessage(
  transaction.compileMessage()
);

console.log(fee, 'lamports');
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
        'method': 'getFeeForMessage',
        'params': ["AQABAgIAAAAMAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAQAA", {"commitment": "processed"}],
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
            "method": "getFeeForMessage",
            "params": ["AQABAgIAAAAMAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAQAA", {"commitment": "processed"}],
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
        "value": 5000
    }
}
```

## Response Parameters

| Parameter | Type   | Description                                                                                  |
| --------- | ------ | -------------------------------------------------------------------------------------------- |
| jsonrpc   | string | JSON-RPC protocol version ("2.0")                                                            |
| id        | string | Request identifier matching the request                                                      |
| result    | object | RPC response object where value is the fee in lamports, or null if the blockhash has expired |

## Use Cases

* **Fee Quotes**: Show the exact lamport cost before a user signs a transaction
* **Batch Sizing**: Compare fees across different instruction counts in one message
* **Fee Payer Checks**: Confirm the fee payer balance covers the quoted amount
* **Blockhash Expiry**: Detect a stale blockhash when the response value is null

## Error Handling

| Error Code | Message                                   | Description                                           |
| ---------- | ----------------------------------------- | ----------------------------------------------------- |
| -32602     | Invalid params                            | Message is not valid base64 or cannot be deserialized |
| -32603     | Internal error                            | Node failed to compute the fee for the message        |
| -32016     | Minimum context slot has not been reached | The node has not yet processed minContextSlot         |
