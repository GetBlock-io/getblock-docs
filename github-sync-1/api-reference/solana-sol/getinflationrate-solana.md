---
description: >-
  Example code for the getInflationRate JSON-RPC method. Complete guide on how
  to use getInflationRate JSON-RPC in GetBlock Web3 documentation.
---

# getInflationRate - Solana

This method returns the inflation rates in effect for the current epoch, split between validator and foundation allocations.

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
    "method": "getInflationRate",
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

const rate = await connection.getInflationRate();

console.log(rate.total, rate.validator, rate.epoch);
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
        'method': 'getInflationRate',
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
            "method": "getInflationRate",
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
        "epoch": 861,
        "foundation": 0.0,
        "total": 0.0432,
        "validator": 0.0432
    }
}
```

## Response Parameters

| Parameter | Type   | Description                                                        |
| --------- | ------ | ------------------------------------------------------------------ |
| jsonrpc   | string | JSON-RPC protocol version ("2.0")                                  |
| id        | string | Request identifier matching the request                            |
| result    | object | Object holding the effective inflation rates for the current epoch |

### Result Object

| Field      | Type   | Description                                                   |
| ---------- | ------ | ------------------------------------------------------------- |
| total      | number | Total annual inflation rate for the epoch                     |
| validator  | number | Portion of inflation distributed to validators and delegators |
| foundation | number | Portion of inflation allocated to the foundation              |
| epoch      | number | Epoch the rates apply to                                      |

## Use Cases

* **APY Display**: Show current nominal staking yield in a staking interface
* **Reward Forecasting**: Estimate next-epoch rewards for a delegated stake position
* **Rate Tracking**: Record inflation drift across epochs as the schedule tapers
* **Dashboard Metrics**: Publish current issuance rate on a network status page

## Error Handling

| Error Code | Message           | Description                                             |
| ---------- | ----------------- | ------------------------------------------------------- |
| -32603     | Internal error    | Node failed to compute the inflation rate for the epoch |
| -32005     | Node is unhealthy | The node has fallen behind the cluster tip              |
