---
description: >-
  Example code for the getEpochSchedule JSON-RPC method. Complete guide on how
  to use the getEpochSchedule JSON-RPC method in the GetBlock Web3
  documentation.
---

# getEpochSchedule - Solana

This method returns the epoch schedule parameters configured at cluster genesis, including epoch length and the warmup behavior applied to early epochs.

## Parameters

This method does not accept any parameters.

## Request

{% tabs %}
{% tab title="cURL" %}
{% code overflow="wrap" %}
```bash
curl --location --request POST 'https://shared.eu-central-1.getblock.io/<ACCESS-TOKEN>/' \
--header 'Content-Type: application/json' \
--data-raw '{
    "jsonrpc": "2.0",
    "method": "getEpochSchedule",
    "params": [],
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

const schedule = await connection.getEpochSchedule();

console.log(schedule.slotsPerEpoch, schedule.firstNormalEpoch);
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
        'method': 'getEpochSchedule',
        'params': [],
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
            "method": "getEpochSchedule",
            "params": [],
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
        "firstNormalEpoch": 14,
        "firstNormalSlot": 524256,
        "leaderScheduleSlotOffset": 432000,
        "slotsPerEpoch": 432000,
        "warmup": true
    }
}
```

## Response Parameters

| Parameter | Type   | Description                                            |
| --------- | ------ | ------------------------------------------------------ |
| jsonrpc   | string | JSON-RPC protocol version ("2.0")                      |
| id        | string | Request identifier matching the request                |
| result    | object | Object holding the cluster's epoch schedule parameters |

### Result Object

| Field                    | Type    | Description                                                             |
| ------------------------ | ------- | ----------------------------------------------------------------------- |
| slotsPerEpoch            | number  | Maximum number of slots in each epoch                                   |
| leaderScheduleSlotOffset | number  | Slots before an epoch starts at which its leader schedule is calculated |
| warmup                   | boolean | Whether epochs started short and grew toward slotsPerEpoch              |
| firstNormalEpoch         | number  | First epoch with the full slotsPerEpoch length                          |
| firstNormalSlot          | number  | Slot at which firstNormalEpoch begins                                   |

## Use Cases

* **Slot to Epoch Math**: Convert an arbitrary slot to its epoch without extra requests
* **Schedule Prefetch**: Time leader schedule fetches using leaderScheduleSlotOffset
* **Historical Analysis**: Handle warmup epochs correctly when indexing early ledger data
* **Test Validators**: Detect shortened epoch settings on a local validator

## Error Handling

| Error Code | Message           | Description                                     |
| ---------- | ----------------- | ----------------------------------------------- |
| -32603     | Internal error    | Node failed to read the requested cluster state |
| -32005     | Node is unhealthy | The node has fallen behind the cluster tip      |
