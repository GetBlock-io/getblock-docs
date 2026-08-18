---
description: >-
  Example code for the getLatestBlockhash JSON-RPC method. Complete guide on how
  to use the getLatestBlockhash JSON-RPC method in the GetBlock Web3
  documentation.
---

# getLatestBlockhash - Solana

This method returns the most recent blockhash and the last block height at which it remains valid. Every transaction must reference a recent blockhash to be accepted by the cluster.

## Parameters

| Parameter | Type   | Required | Description                                 |
| --------- | ------ | -------- | ------------------------------------------- |
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
    "method": "getLatestBlockhash",
    "params": [{"commitment": "confirmed"}],
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

const latestBlockhash = await connection.getLatestBlockhash();

console.log(latestBlockhash.blockhash, latestBlockhash.lastValidBlockHeight);
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
        'method': 'getLatestBlockhash',
        'params': [{"commitment": "confirmed"}],
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
            "method": "getLatestBlockhash",
            "params": [{"commitment": "confirmed"}],
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
            "blockhash": "9zJqLXK9pMhQZ4tGqVvUcxrKpKQ3pMSJ1nZmVeXtBqTd",
            "lastValidBlockHeight": 375812409
        }
    }
}
```

## Response Parameters

| Parameter | Type   | Description                                                                     |
| --------- | ------ | ------------------------------------------------------------------------------- |
| jsonrpc   | string | JSON-RPC protocol version ("2.0")                                               |
| id        | string | Request identifier matching the request                                         |
| result    | object | RPC response object where value holds the blockhash and its expiry block height |

### Value Object

| Field                | Type   | Description                                                |
| -------------------- | ------ | ---------------------------------------------------------- |
| blockhash            | string | Base58-encoded blockhash to reference in a transaction     |
| lastValidBlockHeight | number | Last block height at which the blockhash is still accepted |

## Use Cases

* **Transaction Building**: Attach a recent blockhash before signing and sending
* **Retry Windows**: Stop rebroadcasting once block height passes lastValidBlockHeight
* **Confirmation Strategy**: Pair with a signature subscription for expiry-aware confirmation
* **Durable Nonce Fallback**: Detect when a durable nonce is needed for long signing flows

## Error Handling

| Error Code | Message           | Description                                                   |
| ---------- | ----------------- | ------------------------------------------------------------- |
| -32602     | Invalid params    | Unrecognized commitment level in the config object            |
| -32603     | Internal error    | Node failed to read the recent blockhash queue                |
| -32005     | Node is unhealthy | The node has fallen behind and would return a stale blockhash |
