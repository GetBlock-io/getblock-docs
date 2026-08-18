---
description: >-
  Example code for the getBlockProduction JSON-RPC method. Complete guide on how
  to use getBlockProduction JSON-RPC in GetBlock Web3 documentation.
---

# getBlockProduction - Solana

This method returns the number of slots each validator was assigned as leader and how many blocks it actually produced, over a slot range. It is the basis for validator skip rate calculations.

## Parameters

| Parameter | Type   | Required | Description                                                |
| --------- | ------ | -------- | ---------------------------------------------------------- |
| config    | object | No       | Configuration object controlling range and identity filter |

### Config Object

| Field      | Type   | Required | Description                                                                 |
| ---------- | ------ | -------- | --------------------------------------------------------------------------- |
| commitment | string | No       | Commitment level: processed, confirmed, or finalized. Defaults to finalized |
| identity   | string | No       | Restrict results to a single validator identity Pubkey                      |
| range      | object | No       | Slot range object with firstSlot and optional lastSlot                      |

### Range Object

| Field     | Type   | Required | Description                                                     |
| --------- | ------ | -------- | --------------------------------------------------------------- |
| firstSlot | number | Yes      | First slot of the range, inclusive                              |
| lastSlot  | number | No       | Last slot of the range, inclusive. Defaults to the highest slot |

## Request

{% tabs %}
{% tab title="cURL" %}
{% code overflow="wrap" %}
```bash
curl --location --request POST 'https://shared.eu-central-1.getblock.io/<ACCESS-TOKEN>/' \
--header 'Content-Type: application/json' \
--data-raw '{
    "jsonrpc": "2.0",
    "method": "getBlockProduction",
    "params": [{"identity": "dv1ZAGvdsz5hHLwWXsVnM94hWf1pjbKVau1QVkaMJ92", "range": {"firstSlot": 397229561, "lastSlot": 397234561}}],
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

const { value } = await connection.getBlockProduction({
  identity: 'dv1ZAGvdsz5hHLwWXsVnM94hWf1pjbKVau1QVkaMJ92',
  range: { firstSlot: 397229561, lastSlot: 397234561 }
});

console.log(value.byIdentity);
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
        'method': 'getBlockProduction',
        'params': [{"identity": "dv1ZAGvdsz5hHLwWXsVnM94hWf1pjbKVau1QVkaMJ92", "range": {"firstSlot": 397229561, "lastSlot": 397234561}}],
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
            "method": "getBlockProduction",
            "params": [{"identity": "dv1ZAGvdsz5hHLwWXsVnM94hWf1pjbKVau1QVkaMJ92", "range": {"firstSlot": 397229561, "lastSlot": 397234561}}],
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
        "value": {
            "byIdentity": {
                "dv1ZAGvdsz5hHLwWXsVnM94hWf1pjbKVau1QVkaMJ92": [
                    12,
                    12
                ]
            },
            "range": {
                "firstSlot": 397229561,
                "lastSlot": 397234561
            }
        }
    }
}
```

## Response Parameters

| Parameter | Type   | Description                                                                                  |
| --------- | ------ | -------------------------------------------------------------------------------------------- |
| jsonrpc   | string | JSON-RPC protocol version ("2.0")                                                            |
| id        | string | Request identifier matching the request                                                      |
| result    | object | RPC response object where value holds per-identity production counts and the evaluated range |

### Value Object

| Field      | Type   | Description                                                                          |
| ---------- | ------ | ------------------------------------------------------------------------------------ |
| byIdentity | object | Map of validator identity to a two-element array of leader slots and blocks produced |
| range      | object | The slot range the result covers                                                     |

## Use Cases

* **Skip Rate**: Divide blocks produced by leader slots to score validator reliability
* **Delegation Choice**: Compare production records before delegating stake
* **Uptime Alerts**: Detect a validator that stopped producing within its leader slots
* **Epoch Reports**: Summarize cluster-wide production across a full epoch range

## Error Handling

| Error Code | Message           | Description                                                             |
| ---------- | ----------------- | ----------------------------------------------------------------------- |
| -32602     | Invalid params    | firstSlot is greater than lastSlot, or the identity Pubkey is malformed |
| -32603     | Internal error    | Node failed to read block production data for the range                 |
| -32005     | Node is unhealthy | The node has fallen behind the cluster tip                              |
