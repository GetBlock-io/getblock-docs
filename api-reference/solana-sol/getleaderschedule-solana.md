---
description: >-
  Example code for the getLeaderSchedule JSON-RPC method. Complete guide on how
  to use the getLeaderSchedule JSON-RPC method in the GetBlock Web3
  documentation.
---

# getLeaderSchedule - Solana

This method returns the leader schedule for an epoch, mapping each validator identity to the slot indices it is assigned to lead. Slot indices are relative to the first slot of the epoch.

## Parameters

| Parameter | Type   | Required | Description                                                     |
| --------- | ------ | -------- | --------------------------------------------------------------- |
| slot      | number | No       | Any slot within the target epoch. Defaults to the current epoch |
| config    | object | No       | Configuration object controlling commitment and identity filter |

### Config Object

| Field      | Type   | Required | Description                                                                 |
| ---------- | ------ | -------- | --------------------------------------------------------------------------- |
| commitment | string | No       | Commitment level: processed, confirmed, or finalized. Defaults to finalized |
| identity   | string | No       | Return results for a single validator identity only                         |

## Request

{% tabs %}
{% tab title="cURL" %}
{% code overflow="wrap" %}
```bash
curl --location --request POST 'https://shared.eu-central-1.getblock.io/<ACCESS-TOKEN>/' \
--header 'Content-Type: application/json' \
--data-raw '{
    "jsonrpc": "2.0",
    "method": "getLeaderSchedule",
    "params": [397234561, {"identity": "dv1ZAGvdsz5hHLwWXsVnM94hWf1pjbKVau1QVkaMJ92"}],
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

const schedule = await connection.getLeaderSchedule();

console.log(Object.keys(schedule).length);
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
        'method': 'getLeaderSchedule',
        'params': [397234561, {"identity": "dv1ZAGvdsz5hHLwWXsVnM94hWf1pjbKVau1QVkaMJ92"}],
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
            "method": "getLeaderSchedule",
            "params": [397234561, {"identity": "dv1ZAGvdsz5hHLwWXsVnM94hWf1pjbKVau1QVkaMJ92"}],
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
        "dv1ZAGvdsz5hHLwWXsVnM94hWf1pjbKVau1QVkaMJ92": [
            16,
            17,
            18,
            19,
            2048,
            2049,
            2050,
            2051
        ]
    }
}
```

## Response Parameters

| Parameter | Type   | Description                             |
| --------- | ------ | --------------------------------------- |
| jsonrpc   | string | JSON-RPC protocol version ("2.0")       |
| id        | string | Request identifier matching the request |
| result    | object | null                                    |

## Use Cases

* **Leader-Aware Sending**: Forward transactions to the upcoming leader's TPU to cut latency
* **Slot Assignment**: Show a validator operator when its next leader slots occur
* **Skip Attribution**: Match skipped slots to the identity that was scheduled to lead
* **Stake Distribution**: Compare assigned slot counts as a proxy for stake weight

## Error Handling

| Error Code | Message           | Description                                               |
| ---------- | ----------------- | --------------------------------------------------------- |
| -32602     | Invalid params    | Slot is outside the range of epochs the node can schedule |
| -32603     | Internal error    | Node failed to compute the leader schedule for the epoch  |
| -32005     | Node is unhealthy | The node has fallen behind the cluster tip                |
