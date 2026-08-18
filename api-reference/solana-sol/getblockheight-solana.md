---
description: >-
  Example code for the getBlockHeight JSON-RPC method. Complete guide on how to
  use getBlockHeight JSON-RPC in GetBlock Web3 documentation.
---

# getBlockHeight - Solana

This method returns the current block height, which counts blocks actually produced and excludes skipped slots. Block height is the value transaction expiry is measured against.

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
    "method": "getBlockHeight",
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

const blockHeight = await connection.getBlockHeight();

console.log(blockHeight);
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
        'method': 'getBlockHeight',
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
            "method": "getBlockHeight",
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
    "result": 375812409
}
```

## Response Parameters

| Parameter | Type   | Description                                      |
| --------- | ------ | ------------------------------------------------ |
| jsonrpc   | string | JSON-RPC protocol version ("2.0")                |
| id        | string | Request identifier matching the request          |
| result    | number | Current block height at the requested commitment |

## Use Cases

* **Expiry Tracking**: Compare against lastValidBlockHeight to know when to stop retrying
* **Skip Rate Analysis**: Difference block height against slot to measure skipped slots
* **Sync Monitoring**: Alert when an RPC node's block height lags the cluster
* **Scheduling**: Trigger periodic jobs on block height intervals rather than wall clock

## Error Handling

| Error Code | Message                                   | Description                                        |
| ---------- | ----------------------------------------- | -------------------------------------------------- |
| -32602     | Invalid params                            | Unrecognized commitment level in the config object |
| -32016     | Minimum context slot has not been reached | The node has not yet processed minContextSlot      |
| -32005     | Node is unhealthy                         | The node has fallen behind the cluster tip         |
