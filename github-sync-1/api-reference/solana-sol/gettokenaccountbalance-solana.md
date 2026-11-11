---
description: >-
  Example code for the getTokenAccountBalance JSON-RPC method. Complete guide on
  how to use the getTokenAccountBalance JSON-RPC method in the GetBlock Web3
  documentation.
---

# getTokenAccountBalance - Solana

This method returns the token balance of an SPL Token account. Balances are returned as a raw integer string alongside the mint's decimal precision.

## Parameters

| Parameter | Type   | Required | Description                                    |
| --------- | ------ | -------- | ---------------------------------------------- |
| pubkey    | string | Yes      | Base58-encoded Pubkey of the SPL Token account |
| config    | object | No       | Configuration object controlling commitment    |

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
    "method": "getTokenAccountBalance",
    "params": ["3emsAVdmGKERbHjmGfQ6oZ1e35dkf5z6TmFYcCQnpB2M", {"commitment": "confirmed"}],
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

const { value } = await connection.getTokenAccountBalance(
  new PublicKey('3emsAVdmGKERbHjmGfQ6oZ1e35dkf5z6TmFYcCQnpB2M')
);

console.log(value.uiAmountString);
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
        'method': 'getTokenAccountBalance',
        'params': ["3emsAVdmGKERbHjmGfQ6oZ1e35dkf5z6TmFYcCQnpB2M", {"commitment": "confirmed"}],
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
            "method": "getTokenAccountBalance",
            "params": ["3emsAVdmGKERbHjmGfQ6oZ1e35dkf5z6TmFYcCQnpB2M", {"commitment": "confirmed"}],
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
        "value": {
            "amount": "1250000000",
            "decimals": 6,
            "uiAmount": 1250.0,
            "uiAmountString": "1250"
        }
    }
}
```

## Response Parameters

| Parameter | Type   | Description                                                                  |
| --------- | ------ | ---------------------------------------------------------------------------- |
| jsonrpc   | string | JSON-RPC protocol version ("2.0")                                            |
| id        | string | Request identifier matching the request                                      |
| result    | object | RPC response object where value holds the token amount and decimal precision |

### Value Object

| Field          | Type         | Description                                                   |
| -------------- | ------------ | ------------------------------------------------------------- |
| amount         | string       | Raw token amount as a string, without decimal adjustment      |
| decimals       | number       | Decimal precision defined on the mint                         |
| uiAmount       | number\|null | Amount adjusted for decimals, subject to float precision loss |
| uiAmountString | string       | Amount adjusted for decimals, as a string                     |

## Use Cases

* **Wallet Balances**: Display USDC or USDT balances in a wallet interface
* **Swap Preflight**: Confirm sufficient token balance before building a swap
* **Vault Accounting**: Read protocol vault holdings for TVL calculations
* **Airdrop Checks**: Verify a recipient received an expected token amount

## Error Handling

| Error Code | Message           | Description                                    |
| ---------- | ----------------- | ---------------------------------------------- |
| -32602     | Invalid params    | Pubkey is not an initialized SPL Token account |
| -32603     | Internal error    | Node failed to parse the token account layout  |
| -32005     | Node is unhealthy | The node has fallen behind the cluster tip     |
