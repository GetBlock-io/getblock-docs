---
description: >-
  Example code for the getVoteAccounts JSON-RPC method. Complete guide on how to
  use the getVoteAccounts JSON-RPC method in the GetBlock Web3 documentation.
---

# getVoteAccounts - Solana

This method returns the vote accounts known to the cluster, split into current and delinquent sets. Each entry includes stake, commission, and recent vote credits.

## Parameters

| Parameter | Type   | Required | Description                                             |
| --------- | ------ | -------- | ------------------------------------------------------- |
| config    | object | No       | Configuration object controlling filters and commitment |

### Config Object

| Field                   | Type    | Required | Description                                                                 |
| ----------------------- | ------- | -------- | --------------------------------------------------------------------------- |
| commitment              | string  | No       | Commitment level: processed, confirmed, or finalized. Defaults to finalized |
| votePubkey              | string  | No       | Return results for a single vote account Pubkey only                        |
| keepUnstakedDelinquents | boolean | No       | Include delinquent validators that hold no stake                            |
| delinquentSlotDistance  | number  | No       | Slots behind the tip before a validator is treated as delinquent            |

## Request

{% tabs %}
{% tab title="cURL" %}
{% code overflow="wrap" %}
```bash
curl --location --request POST 'https://shared.eu-central-1.getblock.io/<ACCESS-TOKEN>/' \
--header 'Content-Type: application/json' \
--data-raw '{
    "jsonrpc": "2.0",
    "method": "getVoteAccounts",
    "params": [{"votePubkey": "3ZT31jkAGhUaw8jsy4Q3Q3fSN6r5eYkr4wD1TThTZ4bT"}],
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

const voteAccounts = await connection.getVoteAccounts();

console.log(voteAccounts.current.length, voteAccounts.delinquent.length);
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
        'method': 'getVoteAccounts',
        'params': [{"votePubkey": "3ZT31jkAGhUaw8jsy4Q3Q3fSN6r5eYkr4wD1TThTZ4bT"}],
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
            "method": "getVoteAccounts",
            "params": [{"votePubkey": "3ZT31jkAGhUaw8jsy4Q3Q3fSN6r5eYkr4wD1TThTZ4bT"}],
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
        "current": [
            {
                "activatedStake": 1932847362198,
                "commission": 5,
                "epochCredits": [
                    [
                        859,
                        412893,
                        398201
                    ],
                    [
                        860,
                        428104,
                        412893
                    ]
                ],
                "epochVoteAccount": true,
                "lastVote": 397234560,
                "nodePubkey": "dv1ZAGvdsz5hHLwWXsVnM94hWf1pjbKVau1QVkaMJ92",
                "rootSlot": 397234529,
                "votePubkey": "3ZT31jkAGhUaw8jsy4Q3Q3fSN6r5eYkr4wD1TThTZ4bT"
            }
        ],
        "delinquent": []
    }
}
```

## Response Parameters

| Parameter | Type   | Description                                               |
| --------- | ------ | --------------------------------------------------------- |
| jsonrpc   | string | JSON-RPC protocol version ("2.0")                         |
| id        | string | Request identifier matching the request                   |
| result    | object | Object holding current and delinquent vote account arrays |

### Vote Account Entry

| Field            | Type    | Description                                         |
| ---------------- | ------- | --------------------------------------------------- |
| votePubkey       | string  | Base58 Pubkey of the vote account                   |
| nodePubkey       | string  | Base58 identity Pubkey of the validator             |
| activatedStake   | number  | Stake delegated to this vote account in lamports    |
| commission       | number  | Percentage of rewards taken by the validator        |
| epochVoteAccount | boolean | Whether the account is staked for the current epoch |
| lastVote         | number  | Most recent slot voted on                           |
| rootSlot         | number  | Most recent slot rooted by this validator           |
| epochCredits     | array   | Per-epoch vote credit history                       |

## Use Cases

* **Validator Selection**: Rank validators by stake, commission, and credit history before delegating
* **Delinquency Alerts**: Notify delegators when a validator appears in the delinquent set
* **Nakamoto Coefficient**: Compute stake concentration across the active validator set
* **Stake Pool Management**: Rebalance a pool away from underperforming vote accounts
* **Commission Monitoring**: Detect commission changes that affect delegator returns

## Error Handling

| Error Code | Message           | Description                                                         |
| ---------- | ----------------- | ------------------------------------------------------------------- |
| -32602     | Invalid params    | votePubkey is malformed or delinquentSlotDistance is not an integer |
| -32603     | Internal error    | Node failed to read vote accounts from the bank                     |
| -32005     | Node is unhealthy | The node has fallen behind the cluster tip                          |
