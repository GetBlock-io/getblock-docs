---
description: >-
  Example code for the getBalance JSON-RPC method. Complete guide on how to use
  getBalance JSON-RPC in GetBlock Web3 documentation.
---

# getBalance - Solana

This method returns the lamport balance of an account. Balances are reported in lamports, where 1 SOL equals 1,000,000,000 lamports.

## Parameters

| Parameter | Type   | Required | Description                                   |
| --------- | ------ | -------- | --------------------------------------------- |
| pubkey    | string | Yes      | Base58-encoded Pubkey of the account to query |
| config    | object | No       | Configuration object controlling commitment   |

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
    "method": "getBalance",
    "params": ["EPjFWdd5AufqSSqeM2qN1xzybapC8G4wEGGkZwyTDt1v", {"commitment": "finalized"}],
    "id": "getblock.io"
}'
```
{% endcode %}
{% endtab %}

{% tab title="@solana/web3.js" %}
{% code title="example.js" %}
```javascript
const { Connection, PublicKey, LAMPORTS_PER_SOL } = require('@solana/web3.js');

const connection = new Connection('https://shared.eu-central-1.getblock.io/<ACCESS-TOKEN>/', 'finalized');

const lamports = await connection.getBalance(
  new PublicKey('EPjFWdd5AufqSSqeM2qN1xzybapC8G4wEGGkZwyTDt1v')
);

console.log(lamports / LAMPORTS_PER_SOL, 'SOL');
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
        'method': 'getBalance',
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
            "method": "getBalance",
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
        "value": 389532986339
    }
}
```

## Response Parameters

| Parameter | Type   | Description                                                |
| --------- | ------ | ---------------------------------------------------------- |
| jsonrpc   | string | JSON-RPC protocol version ("2.0")                          |
| id        | string | Request identifier matching the request                    |
| result    | object | RPC response object where value is the balance in lamports |

## Use Cases

* **Wallet Balances**: Display SOL balances in wallet and portfolio interfaces
* **Fee Preflight**: Confirm a fee payer holds enough lamports before signing
* **Rent Checks**: Compare a balance against the rent-exempt minimum
* **Faucet Automation**: Trigger a devnet airdrop when a test account runs dry
* **Treasury Monitoring**: Poll protocol treasury accounts for balance changes

## Error Handling

| Error Code | Message                                   | Description                                                               |
| ---------- | ----------------------------------------- | ------------------------------------------------------------------------- |
| -32602     | Invalid params                            | Malformed base58 Pubkey, unsupported encoding, or invalid dataSlice range |
| -32603     | Internal error                            | Node failed to read the account from its accounts database                |
| -32016     | Minimum context slot has not been reached | The node has not yet processed minContextSlot                             |
| -32005     | Node is unhealthy                         | The node has fallen behind the cluster tip and cannot serve account state |
