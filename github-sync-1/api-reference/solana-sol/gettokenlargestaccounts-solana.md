---
description: >-
  Example code for the getTokenLargestAccounts JSON-RPC method. Complete guide
  on how to use the getTokenLargestAccounts JSON-RPC method in the GetBlock Web3
  documentation.
---

# getTokenLargestAccounts - Solana

This method returns the 20 largest token accounts for an SPL Token mint, ordered by balance. Results reflect token accounts only, not the wallet addresses that own them.

## Parameters

| Parameter | Type   | Required | Description                                 |
| --------- | ------ | -------- | ------------------------------------------- |
| mint      | string | Yes      | Base58-encoded Pubkey of the SPL Token mint |
| config    | object | No       | Configuration object controlling commitment |

### Config Object

| Field      | Type   | Required | Description                                                                 |
| ---------- | ------ | -------- | --------------------------------------------------------------------------- |
| commitment | string | No       | Commitment level: processed, confirmed, or finalized. Defaults to finalized |

## Request

{% tabs %}
{% tab title="cURL" %}
{% code overflow="wrap" %}
```bash
curl --location --request POST 'https://shared.eu-central-1.getblock.io/<ACCESS-TOKEN>/' \
--header 'Content-Type: application/json' \
--data-raw '{
    "jsonrpc": "2.0",
    "method": "getTokenLargestAccounts",
    "params": ["EPjFWdd5AufqSSqeM2qN1xzybapC8G4wEGGkZwyTDt1v", {"commitment": "finalized"}],
    "id": "getblock.io"
}'
```
{% endcode %}
{% endtab %}

{% tab title="@solana/web3.js" %}
{% code title="example.js" %}
```javascript
const { Connection, PublicKey } = require('@solana/web3.js');

const connection = new Connection('https://shared.eu-central-1.getblock.io/<ACCESS-TOKEN>/', 'finalized');

const { value } = await connection.getTokenLargestAccounts(
  new PublicKey('EPjFWdd5AufqSSqeM2qN1xzybapC8G4wEGGkZwyTDt1v')
);

console.log(value);
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
        'method': 'getTokenLargestAccounts',
        'params': ["EPjFWdd5AufqSSqeM2qN1xzybapC8G4wEGGkZwyTDt1v", {"commitment": "finalized"}],
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
            "method": "getTokenLargestAccounts",
            "params": ["EPjFWdd5AufqSSqeM2qN1xzybapC8G4wEGGkZwyTDt1v", {"commitment": "finalized"}],
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
            "apiVersion": "2.3.6",
            "slot": 397234561
        },
        "value": [
            {
                "address": "3emsAVdmGKERbHjmGfQ6oZ1e35dkf5z6TmFYcCQnpB2M",
                "amount": "184203771290000",
                "decimals": 6,
                "uiAmount": 184203771.29,
                "uiAmountString": "184203771.29"
            }
        ]
    }
}
```

## Response Parameters

| Parameter | Type   | Description                                                                  |
| --------- | ------ | ---------------------------------------------------------------------------- |
| jsonrpc   | string | JSON-RPC protocol version ("2.0")                                            |
| id        | string | Request identifier matching the request                                      |
| result    | object | RPC response object where value is an array of the 20 largest token accounts |

### Value Entry

| Field          | Type   | Description                               |
| -------------- | ------ | ----------------------------------------- |
| address        | string | Base58 Pubkey of the token account        |
| amount         | string | Raw token amount as a string              |
| decimals       | number | Decimal precision defined on the mint     |
| uiAmount       | number | null                                      |
| uiAmountString | string | Amount adjusted for decimals, as a string |

## Use Cases

* **Holder Concentration**: Assess how concentrated a token's supply is before listing it
* **Liquidity Mapping**: Identify AMM pool vaults holding the largest balances
* **Rug Screening**: Check whether a small set of accounts controls most of the supply
* **Treasury Discovery**: Locate protocol-owned token accounts for a given mint

## Error Handling

| Error Code | Message           | Description                                     |
| ---------- | ----------------- | ----------------------------------------------- |
| -32602     | Invalid params    | Pubkey is not an initialized SPL Token mint     |
| -32603     | Internal error    | Node failed to scan token accounts for the mint |
| -32005     | Node is unhealthy | The node has fallen behind the cluster tip      |
