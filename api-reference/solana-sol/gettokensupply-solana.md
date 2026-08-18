---
description: >-
  Example code for the getTokenSupply JSON-RPC method. Complete guide on how to
  use the getTokenSupply JSON-RPC method in the GetBlock Web3 documentation.
---

# getTokenSupply - Solana

This method returns the total circulating supply of an SPL Token mint, together with the mint's decimal precision.

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
    "method": "getTokenSupply",
    "params": ["EPjFWdd5AufqSSqeM2qN1xzybapC8G4wEGGkZwyTDt1v", {"commitment": "finalized"}],
    "id": "getblock.io"
}'
```
{% endcode %}
{% endtab %}

{% tab title="@solana/web3.js" %}
```javascript
const { Connection, PublicKey } = require('@solana/web3.js');

const connection = new Connection('https://shared.eu-central-1.getblock.io/<ACCESS-TOKEN>/', 'finalized');

const { value } = await connection.getTokenSupply(
  new PublicKey('EPjFWdd5AufqSSqeM2qN1xzybapC8G4wEGGkZwyTDt1v')
);

console.log(value.uiAmountString);
```
{% endtab %}

{% tab title="Request" %}
```python
import requests

response = requests.post(
    'https://shared.eu-central-1.getblock.io/<ACCESS-TOKEN>/',
    headers={'Content-Type': 'application/json'},
    json={
        'jsonrpc': '2.0',
        'method': 'getTokenSupply',
        'params': ["EPjFWdd5AufqSSqeM2qN1xzybapC8G4wEGGkZwyTDt1v", {"commitment": "finalized"}],
        'id': 'getblock.io'
    }
)

print(response.json()['result'])
```
{% endtab %}

{% tab title="Rust" %}
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
            "method": "getTokenSupply",
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
        "value": {
            "amount": "8721043118453221",
            "decimals": 6,
            "uiAmount": 8721043118.45322,
            "uiAmountString": "8721043118.453221"
        }
    }
}
```

## Response Parameters

| Parameter | Type   | Description                                                                 |
| --------- | ------ | --------------------------------------------------------------------------- |
| jsonrpc   | string | JSON-RPC protocol version ("2.0")                                           |
| id        | string | Request identifier matching the request                                     |
| result    | object | RPC response object where value holds the mint supply and decimal precision |

### Value Object

| Field          | Type   | Description                                        |
| -------------- | ------ | -------------------------------------------------- |
| amount         | string | Raw supply as a string, without decimal adjustment |
| decimals       | number | Decimal precision defined on the mint              |
| uiAmount       | number | null                                               |
| uiAmountString | string | Supply adjusted for decimals, as a string          |

## Use Cases

* **Market Cap**: Multiply circulating supply by price to compute market capitalization
* **Mint Monitoring**: Detect supply increases that indicate new minting activity
* **Burn Verification**: Confirm supply decreased after a burn instruction
* **Stablecoin Reporting**: Track issued supply of USDC and USDT on Solana

## Error Handling

| Error Code | Message           | Description                                 |
| ---------- | ----------------- | ------------------------------------------- |
| -32602     | Invalid params    | Pubkey is not an initialized SPL Token mint |
| -32603     | Internal error    | Node failed to read the mint account        |
| -32005     | Node is unhealthy | The node has fallen behind the cluster tip  |
