---
description: >-
  Example code for the getBlockTime JSON-RPC method. Complete guide on how to
  use the getBlockTime JSON-RPC method in the GetBlock Web3 documentation.
---

# getBlockTime - Solana

This method returns the estimated production time of a block as a Unix timestamp. The estimate is derived from stake-weighted validator timestamp oracles.

## Parameters

| Parameter | Type   | Required | Description                       |
| --------- | ------ | -------- | --------------------------------- |
| slot      | number | Yes      | Slot number of the block to query |

## Request

{% tabs %}
{% tab title="cURL" %}
{% code overflow="wrap" %}
```bash
curl --location --request POST 'https://shared.eu-central-1.getblock.io/<ACCESS-TOKEN>/' \
--header 'Content-Type: application/json' \
--data-raw '{
    "jsonrpc": "2.0",
    "method": "getBlockTime",
    "params": [397234561],
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

const blockTime = await connection.getBlockTime(397234561);

console.log(new Date(blockTime * 1000).toISOString());
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
        'method': 'getBlockTime',
        'params': [397234561],
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
            "method": "getBlockTime",
            "params": [397234561],
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
    "result": 1774267845
}
```

## Response Parameters

| Parameter | Type   | Description                             |
| --------- | ------ | --------------------------------------- |
| jsonrpc   | string | JSON-RPC protocol version ("2.0")       |
| id        | string | Request identifier matching the request |
| result    | number | null                                    |

## Use Cases

* **Timestamped History**: Label transaction history entries with wall clock time
* **Interval Analysis**: Measure real time between two slots for latency reporting
* **Tax Reporting**: Attach dates to on-chain transfers for accounting exports
* **Cache Keys**: Bucket indexed data by day using block timestamps

## Error Handling

| Error Code | Message                                                            | Description                                                        |
| ---------- | ------------------------------------------------------------------ | ------------------------------------------------------------------ |
| -32007     | Slot was skipped, or missing due to ledger jump to recent snapshot | The slot was skipped by the leader or predates the node's snapshot |
| -32009     | Slot was skipped, or missing in long-term storage                  | The slot is absent from the node's long-term ledger storage        |
| -32602     | Invalid params                                                     | Slot is negative or not an integer                                 |
| -32004     | Block not available for slot                                       | The node has not yet produced or received the block                |
