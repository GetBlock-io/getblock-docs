---
description: >-
  Example code for the getSlot JSON-RPC method. Complete guide on how to use the
  getSlot JSON-RPC method in the GetBlock Web3 documentation.
---

# getSlot - Solana

This method returns the highest slot the node has reached at the requested commitment level. Slots increment on a fixed schedule whether or not a block is produced.

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
    "method": "getSlot",
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

const slot = await connection.getSlot();

console.log(slot);
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
        'method': 'getSlot',
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
            "method": "getSlot",
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
    "result": 397234561
}
```

## Response Parameters

| Parameter | Type   | Description                                      |
| --------- | ------ | ------------------------------------------------ |
| jsonrpc   | string | JSON-RPC protocol version ("2.0")                |
| id        | string | Request identifier matching the request          |
| result    | number | Highest slot reached at the requested commitment |

## Use Cases

* **Sync Checks**: Compare slots across providers to detect a lagging endpoint
* **Polling Anchors**: Use the current slot as a cursor for incremental ingestion
* **Commitment Gaps**: Measure the distance between processed and finalized slots
* **Timing Estimates**: Convert slot deltas to elapsed time at roughly 400 ms per slot

## Error Handling

| Error Code | Message                                   | Description                                        |
| ---------- | ----------------------------------------- | -------------------------------------------------- |
| -32602     | Invalid params                            | Unrecognized commitment level in the config object |
| -32016     | Minimum context slot has not been reached | The node has not yet processed minContextSlot      |
| -32603     | Internal error                            | Node failed to read the requested cluster state    |
| -32005     | Node is unhealthy                         | The node has fallen behind the cluster tip         |
