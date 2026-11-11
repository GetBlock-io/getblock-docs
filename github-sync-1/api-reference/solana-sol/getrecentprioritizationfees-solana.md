---
description: >-
  Example code for the getRecentPrioritizationFees JSON-RPC method. Complete
  guide on how to use the getRecentPrioritizationFees JSON-RPC method in the
  GetBlock Web3 documentation.
---

# getRecentPrioritizationFees - Solana

This method returns a list of prioritization fees observed in recent slots, optionally scoped to transactions that lock a given set of accounts. Prioritization fees are paid per compute unit on top of the base fee.

## Parameters

| Parameter | Type  | Required | Description                                                           |
| --------- | ----- | -------- | --------------------------------------------------------------------- |
| addresses | array | No       | Array of up to 128 base58-encoded account Pubkeys to scope the sample |

## Request

{% tabs %}
{% tab title="cURL" %}
{% code overflow="wrap" %}
```bash
curl --location --request POST 'https://shared.eu-central-1.getblock.io/<ACCESS-TOKEN>/' \
--header 'Content-Type: application/json' \
--data-raw '{
    "jsonrpc": "2.0",
    "method": "getRecentPrioritizationFees",
    "params": [["JUP6LkbZbjS1jKKwapdHNy74zcZ3tLUZoi5QNyVTaV4"]],
    "id": "getblock.io"
}'
```
{% endcode %}
{% endtab %}

{% tab title="@solana/web3.js" %}
{% code title="example.js" %}
```javascript
const { Connection, PublicKey } = require('@solana/web3.js');

const connection = new Connection('https://shared.eu-central-1.getblock.io/<ACCESS-TOKEN>/', 'confirmed');

const fees = await connection.getRecentPrioritizationFees({
  lockedWritableAccounts: [new PublicKey('JUP6LkbZbjS1jKKwapdHNy74zcZ3tLUZoi5QNyVTaV4')]
});

console.log(fees.slice(0, 5));
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
        'method': 'getRecentPrioritizationFees',
        'params': [["JUP6LkbZbjS1jKKwapdHNy74zcZ3tLUZoi5QNyVTaV4"]],
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
            "method": "getRecentPrioritizationFees",
            "params": [["JUP6LkbZbjS1jKKwapdHNy74zcZ3tLUZoi5QNyVTaV4"]],
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
        {
            "slot": 397234560,
            "prioritizationFee": 0
        },
        {
            "slot": 397234561,
            "prioritizationFee": 12500
        }
    ]
}
```

## Response Parameters

| Parameter | Type   | Description                                  |
| --------- | ------ | -------------------------------------------- |
| jsonrpc   | string | JSON-RPC protocol version ("2.0")            |
| id        | string | Request identifier matching the request      |
| result    | array  | Array of per-slot prioritization fee samples |

### Sample Entry

| Field             | Type   | Description                                              |
| ----------------- | ------ | -------------------------------------------------------- |
| slot              | number | Slot the fee sample was taken from                       |
| prioritizationFee | number | Fee in micro-lamports per compute unit paid in that slot |

## Use Cases

* **Dynamic Fee Bidding**: Set a compute unit price from recent percentiles instead of a fixed value
* **Congestion Detection**: Identify hot accounts where writes are being contested
* **Swap Routing**: Price prioritization into quotes before submitting to a DEX program
* **Cost Dashboards**: Chart fee pressure per program over recent slots

## Error Handling

| Error Code | Message        | Description                                            |
| ---------- | -------------- | ------------------------------------------------------ |
| -32602     | Invalid params | More than 128 addresses supplied or a malformed Pubkey |
| -32603     | Internal error | Node failed to read the prioritization fee cache       |
