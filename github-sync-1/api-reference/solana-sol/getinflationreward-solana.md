---
description: >-
  Example code for the getInflationReward JSON-RPC method. Complete guide on how
  to use getInflationReward JSON-RPC in GetBlock Web3 documentation.
---

# getInflationReward - Solana

This method returns the inflation reward credited to a list of addresses for a given epoch. Addresses that received no reward return null.

## Parameters

| Parameter | Type   | Required | Description                                           |
| --------- | ------ | -------- | ----------------------------------------------------- |
| addresses | array  | Yes      | Array of base58-encoded addresses to query            |
| config    | object | No       | Configuration object controlling epoch and commitment |

### Config Object

| Field          | Type   | Required | Description                                                                 |
| -------------- | ------ | -------- | --------------------------------------------------------------------------- |
| commitment     | string | No       | Commitment level: processed, confirmed, or finalized. Defaults to finalized |
| epoch          | number | No       | Epoch to query rewards for. Defaults to the previous epoch                  |
| minContextSlot | number | No       | Minimum slot the request can be evaluated at                                |

## Request

{% tabs %}
{% tab title="cURL" %}
```bash
curl --location --request POST 'https://shared.eu-central-1.getblock.io/<ACCESS-TOKEN>/' \
--header 'Content-Type: application/json' \
--data-raw '{
    "jsonrpc": "2.0",
    "method": "getInflationReward",
    "params": [["3ZT31jkAGhUaw8jsy4Q3Q3fSN6r5eYkr4wD1TThTZ4bT"], {"epoch": 860}],
    "id": "getblock.io"
}'
```
{% endtab %}

{% tab title="@solana/web3.js" %}
{% code title="example.js" %}
```javascript
const { Connection, PublicKey } = require('@solana/web3.js');

const connection = new Connection('https://shared.eu-central-1.getblock.io/<ACCESS-TOKEN>/', 'confirmed');

const rewards = await connection.getInflationReward(
  [new PublicKey('3ZT31jkAGhUaw8jsy4Q3Q3fSN6r5eYkr4wD1TThTZ4bT')],
  860
);

console.log(rewards[0]);
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
        'method': 'getInflationReward',
        'params': [["3ZT31jkAGhUaw8jsy4Q3Q3fSN6r5eYkr4wD1TThTZ4bT"], {"epoch": 860}],
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
            "method": "getInflationReward",
            "params": [["3ZT31jkAGhUaw8jsy4Q3Q3fSN6r5eYkr4wD1TThTZ4bT"], {"epoch": 860}],
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
            "amount": 1893472,
            "commission": 5,
            "effectiveSlot": 397234461,
            "epoch": 860,
            "postBalance": 502893472
        }
    ]
}
```

## Response Parameters

| Parameter | Type   | Description                                                                     |
| --------- | ------ | ------------------------------------------------------------------------------- |
| jsonrpc   | string | JSON-RPC protocol version ("2.0")                                               |
| id        | string | Request identifier matching the request                                         |
| result    | array  | Array of reward objects or null entries, in the order of the supplied addresses |

### Reward Entry

| Field         | Type   | Description                                  |
| ------------- | ------ | -------------------------------------------- |
| epoch         | number | Epoch the reward was credited for            |
| effectiveSlot | number | Slot at which the reward became effective    |
| amount        | number | Reward amount in lamports                    |
| postBalance   | number | Account balance in lamports after the reward |
| commission    | number | null                                         |

## Use Cases

* **Staking Statements**: Report per-epoch rewards to delegators in a staking dashboard
* **Realized APY**: Divide reward amount by stake to compute actual yield per epoch
* **Commission Audits**: Verify a validator applied the commission it advertises
* **Tax Exports**: Produce epoch-by-epoch reward records for accounting

## Error Handling

| Error Code | Message                                             | Description                                                  |
| ---------- | --------------------------------------------------- | ------------------------------------------------------------ |
| -32602     | Invalid params                                      | Epoch is in the future or an address is malformed            |
| -32011     | Transaction history is not available from this node | The node does not retain reward data for the requested epoch |
| -32603     | Internal error                                      | Node failed to read the epoch rewards partition              |
