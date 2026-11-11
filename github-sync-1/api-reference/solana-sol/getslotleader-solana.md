---
description: >-
  Example code for the getSlotLeader JSON-RPC method. Complete guide on how to
  use the getSlotLeader JSON-RPC method in the GetBlock Web3 documentation.
---

# getSlotLeader - Solana

This method returns the identity Pubkey of the validator scheduled to produce the block for the current slot at the requested commitment.

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
    "method": "getSlotLeader",
    "params": [{"commitment": "processed"}],
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

const leader = await connection.getSlotLeader();

console.log(leader);
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
        'method': 'getSlotLeader',
        'params': [{"commitment": "processed"}],
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
            "method": "getSlotLeader",
            "params": [{"commitment": "processed"}],
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
    "result": "dv1ZAGvdsz5hHLwWXsVnM94hWf1pjbKVau1QVkaMJ92"
}
```

## Response Parameters

| Parameter | Type   | Description                                               |
| --------- | ------ | --------------------------------------------------------- |
| jsonrpc   | string | JSON-RPC protocol version ("2.0")                         |
| id        | string | Request identifier matching the request                   |
| result    | string | Base58-encoded identity Pubkey of the current slot leader |

## Use Cases

* **Direct TPU Sends**: Route a transaction to the current leader instead of a random node
* **MEV Research**: Attribute block contents to the identity that produced them
* **Live Dashboards**: Display the active leader on a network monitoring page
* **Latency Studies**: Correlate confirmation times with specific leaders

## Error Handling

| Error Code | Message                                   | Description                                        |
| ---------- | ----------------------------------------- | -------------------------------------------------- |
| -32602     | Invalid params                            | Unrecognized commitment level in the config object |
| -32016     | Minimum context slot has not been reached | The node has not yet processed minContextSlot      |
| -32603     | Internal error                            | Node failed to read the requested cluster state    |
| -32005     | Node is unhealthy                         | The node has fallen behind the cluster tip         |
