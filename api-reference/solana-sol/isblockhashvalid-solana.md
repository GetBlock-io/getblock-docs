---
description: >-
  Example code for the isBlockhashValid JSON-RPC method. Complete guide on how
  to use the isBlockhashValid JSON-RPC method in the GetBlock Web3
  documentation.
---

# isBlockhashValid - Solana

This method reports whether a blockhash is still recent enough to be accepted by the cluster. Blockhashes expire roughly 150 slots after they are produced.

## Parameters

| Parameter | Type   | Required | Description                                 |
| --------- | ------ | -------- | ------------------------------------------- |
| blockhash | string | Yes      | Base58-encoded blockhash to check           |
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
    "method": "isBlockhashValid",
    "params": ["9zJqLXK9pMhQZ4tGqVvUcxrKpKQ3pMSJ1nZmVeXtBqTd", {"commitment": "processed"}],
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

const isValid = await connection.isBlockhashValid('9zJqLXK9pMhQZ4tGqVvUcxrKpKQ3pMSJ1nZmVeXtBqTd');

console.log(isValid);
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
        'method': 'isBlockhashValid',
        'params': ["9zJqLXK9pMhQZ4tGqVvUcxrKpKQ3pMSJ1nZmVeXtBqTd", {"commitment": "processed"}],
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
            "method": "isBlockhashValid",
            "params": ["9zJqLXK9pMhQZ4tGqVvUcxrKpKQ3pMSJ1nZmVeXtBqTd", {"commitment": "processed"}],
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
        "value": true
    }
}
```

## Response Parameters

| Parameter | Type   | Description                                                               |
| --------- | ------ | ------------------------------------------------------------------------- |
| jsonrpc   | string | JSON-RPC protocol version ("2.0")                                         |
| id        | string | Request identifier matching the request                                   |
| result    | object | RPC response object where value is true when the blockhash is still valid |

## Use Cases

* **Pre-Send Checks**: Avoid submitting a transaction built on an expired blockhash
* **Retry Loops**: Stop rebroadcasting once the referenced blockhash expires
* **Offline Signing**: Validate a blockhash after a delay between signing and submission
* **Queue Draining**: Discard queued transactions whose blockhashes have lapsed

## Error Handling

| Error Code | Message        | Description                                           |
| ---------- | -------------- | ----------------------------------------------------- |
| -32602     | Invalid params | Blockhash is not valid base58 or has the wrong length |
| -32603     | Internal error | Node failed to read the blockhash queue               |
