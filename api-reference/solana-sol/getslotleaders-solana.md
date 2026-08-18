---
description: >-
  Example code for the getSlotLeaders JSON-RPC method. Complete guide on how to
  use the getSlotLeaders JSON-RPC method in the GetBlock Web3 documentation.
---

# getSlotLeaders - Solana

This method returns the identity Pubkey of the scheduled leader for each slot in a contiguous range. A maximum of 5,000 slots can be requested per call.

## Parameters

| Parameter | Type   | Required | Description                                   |
| --------- | ------ | -------- | --------------------------------------------- |
| startSlot | number | Yes      | First slot of the range, inclusive            |
| limit     | number | Yes      | Number of slots to return, between 1 and 5000 |

## Request

{% tabs %}
{% tab title="cURL" %}
{% code overflow="wrap" %}
```bash
curl --location --request POST 'https://shared.eu-central-1.getblock.io/<ACCESS-TOKEN>/' \
--header 'Content-Type: application/json' \
--data-raw '{
    "jsonrpc": "2.0",
    "method": "getSlotLeaders",
    "params": [397234561, 4],
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

const leaders = await connection.getSlotLeaders(397234561, 4);

console.log(leaders.map((l) => l.toBase58()));
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
        'method': 'getSlotLeaders',
        'params': [397234561, 4],
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
            "method": "getSlotLeaders",
            "params": [397234561, 4],
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
    "result": [
        "dv1ZAGvdsz5hHLwWXsVnM94hWf1pjbKVau1QVkaMJ92",
        "dv1ZAGvdsz5hHLwWXsVnM94hWf1pjbKVau1QVkaMJ92",
        "dv1ZAGvdsz5hHLwWXsVnM94hWf1pjbKVau1QVkaMJ92",
        "dv1ZAGvdsz5hHLwWXsVnM94hWf1pjbKVau1QVkaMJ92"
    ]
}
```

## Response Parameters

| Parameter | Type   | Description                                                           |
| --------- | ------ | --------------------------------------------------------------------- |
| jsonrpc   | string | JSON-RPC protocol version ("2.0")                                     |
| id        | string | Request identifier matching the request                               |
| result    | array  | Array of leader identity Pubkeys, one per slot in the requested range |

## Use Cases

* **Leader Prefetch**: Warm connections to the next few leaders before sending
* **Send Fanout**: Broadcast a transaction to several upcoming leaders at once
* **Rotation Analysis**: Observe the four-slot leader rotation pattern directly
* **Operator Alerts**: Notify an operator shortly before its leader slots begin

## Error Handling

| Error Code | Message           | Description                                                       |
| ---------- | ----------------- | ----------------------------------------------------------------- |
| -32602     | Invalid params    | Limit exceeds 5000 or the range extends beyond the known schedule |
| -32603     | Internal error    | Node failed to read the leader schedule for the range             |
| -32005     | Node is unhealthy | The node has fallen behind the cluster tip                        |
