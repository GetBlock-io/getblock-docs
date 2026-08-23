---
description: >-
  Example code for the sendrawtransaction JSON-RPC method. Complete guide on how
  to use sendrawtransaction JSON-RPC in GetBlock Web3 documentation.
---

# sendrawtransaction - Bitcoin

This method submits a raw, fully signed transaction (serialized as hex) to the network. On success it returns the transaction ID.

## Parameters

| Parameter  | Type             | Required | Description                                                                             |
| ---------- | ---------------- | -------- | --------------------------------------------------------------------------------------- |
| hexstring  | string           | Yes      | The signed transaction, serialized as hex                                               |
| maxfeerate | number or string | No       | Reject if the fee rate exceeds this value (default: 0.10 BTC/kvB); 0 disables the check |

## Request

{% tabs %}
{% tab title="cURL" %}
{% code overflow="wrap" %}
```bash
curl --location --request POST 'https://shared.eu-central-1.getblock.io/<ACCESS-TOKEN>/' \
--header 'Content-Type: application/json' \
--data-raw '{
    "jsonrpc": "2.0",
    "method": "sendrawtransaction",
    "params": ["0200000001a1b2c3d4e5f6a7b8c9d0e1f2a3b4c5d6e7f8091a2b3c4d5e6f708192a3b4c5d60000000000ffffffff0100e1f505000000001600141d0f172a0ecb48aee1be1f2687d2963ae33f71a100000000"],
    "id": "getblock.io"
}'
```
{% endcode %}
{% endtab %}

{% tab title="bitcoinjs-lib" %}
{% code title="example.js" %}
```javascript
const axios = require('axios');

const response = await axios.post('https://shared.eu-central-1.getblock.io/<ACCESS-TOKEN>/', {
    jsonrpc: '2.0',
    method: 'sendrawtransaction',
    params: ["0200000001a1b2c3d4e5f6a7b8c9d0e1f2a3b4c5d6e7f8091a2b3c4d5e6f708192a3b4c5d60000000000ffffffff0100e1f505000000001600141d0f172a0ecb48aee1be1f2687d2963ae33f71a100000000"],
    id: 'getblock.io'
}, {
    headers: { 'Content-Type': 'application/json' }
});

console.log(response.data.result);
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
        'method': 'sendrawtransaction',
        'params': ["0200000001a1b2c3d4e5f6a7b8c9d0e1f2a3b4c5d6e7f8091a2b3c4d5e6f708192a3b4c5d60000000000ffffffff0100e1f505000000001600141d0f172a0ecb48aee1be1f2687d2963ae33f71a100000000"],
        'id': 'getblock.io'
    }
)

print(response.json())
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
            "method": "sendrawtransaction",
            "params": ["0200000001a1b2c3d4e5f6a7b8c9d0e1f2a3b4c5d6e7f8091a2b3c4d5e6f708192a3b4c5d60000000000ffffffff0100e1f505000000001600141d0f172a0ecb48aee1be1f2687d2963ae33f71a100000000"],
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
    "result": "4a5e1e4baab89f3a32518a88c31bc87f618f76673e2cc77ab2127b7afdeda33b"
}
```

## Response Parameters

| Parameter | Type   | Description                                     |
| --------- | ------ | ----------------------------------------------- |
| jsonrpc   | string | JSON-RPC protocol version ("2.0")               |
| id        | string | Request identifier matching the request         |
| result    | string | The transaction ID of the broadcast transaction |

## Use Cases

* **Transaction Broadcast**: Submit a signed transaction to the network
* **Wallet Backends**: Broadcast transactions built off-node
* **Custody Flows**: Broadcast externally signed transactions
* **Automation**: Send scheduled or batched transactions

## Error Handling

| Error Code | Message          | Description                                                           |
| ---------- | ---------------- | --------------------------------------------------------------------- |
| 403        | Forbidden        | Missing or invalid ACCESS-TOKEN                                       |
| -25        | Verify error     | The transaction failed script or consensus verification               |
| -26        | Rejected         | The transaction was rejected by mempool policy (for example, low fee) |
| -27        | Already in chain | The transaction is already in a block                                 |
