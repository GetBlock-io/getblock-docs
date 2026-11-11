---
description: >-
  Example code for the getEpochInfo JSON-RPC method. Complete guide on how to
  use the getEpochInfo JSON-RPC method in the GetBlock Web3 documentation.
---

# getEpochInfo - Solana

This method returns the cluster's position within the current epoch, including the current slot, epoch number, and progress through the epoch's slot range.

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
    "method": "getEpochInfo",
    "params": [{"commitment": "finalized"}],
    "id": "getblock.io"
}'
```
{% endcode %}
{% endtab %}

{% tab title="@solana/web3.js" %}
{% code title="example.js" %}
```javascript
const { Connection } = require('@solana/web3.js');

const connection = new Connection('https://shared.eu-central-1.getblock.io/<ACCESS-TOKEN>/', 'finalized');

const epochInfo = await connection.getEpochInfo();

console.log(epochInfo.epoch, epochInfo.slotIndex, epochInfo.slotsInEpoch);
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
        'method': 'getEpochInfo',
        'params': [{"commitment": "finalized"}],
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
            "method": "getEpochInfo",
            "params": [{"commitment": "finalized"}],
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
        "absoluteSlot": 397234561,
        "blockHeight": 375812409,
        "epoch": 861,
        "slotIndex": 271361,
        "slotsInEpoch": 432000,
        "transactionCount": 412873094521
    }
}
```

## Response Parameters

| Parameter | Type   | Description                                            |
| --------- | ------ | ------------------------------------------------------ |
| jsonrpc   | string | JSON-RPC protocol version ("2.0")                      |
| id        | string | Request identifier matching the request                |
| result    | object | Object describing the cluster's current epoch position |

### Result Object

| Field            | Type   | Description                                      |
| ---------------- | ------ | ------------------------------------------------ |
| absoluteSlot     | number | Current slot since genesis                       |
| blockHeight      | number | Current block height, excluding skipped slots    |
| epoch            | number | Current epoch number                             |
| slotIndex        | number | Slot position relative to the start of the epoch |
| slotsInEpoch     | number | Total slots in the current epoch                 |
| transactionCount | number | null                                             |

## Use Cases

* **Epoch Countdown**: Estimate time to epoch end from slotIndex and slot duration
* **Reward Timing**: Schedule reward distribution jobs around epoch boundaries
* **Stake Activation**: Predict when a delegation becomes fully active
* **Dashboard Headers**: Display epoch progress on a validator or staking interface

## Error Handling

| Error Code | Message                                   | Description                                        |
| ---------- | ----------------------------------------- | -------------------------------------------------- |
| -32602     | Invalid params                            | Unrecognized commitment level in the config object |
| -32016     | Minimum context slot has not been reached | The node has not yet processed minContextSlot      |
| -32603     | Internal error                            | Node failed to read the requested cluster state    |
| -32005     | Node is unhealthy                         | The node has fallen behind the cluster tip         |
