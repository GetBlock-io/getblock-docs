---
description: >-
  Example code for the requestAirdrop JSON-RPC method. Complete guide on how to
  use the requestAirdrop JSON-RPC method in the GetBlock Web3 documentation.
---

# requestAirdrop - Solana

This method requests lamports from the cluster faucet and returns the resulting transaction signature. Airdrops are available on Devnet and Testnet only.

## Parameters

| Parameter | Type   | Required | Description                                  |
| --------- | ------ | -------- | -------------------------------------------- |
| pubkey    | string | Yes      | Base58-encoded Pubkey of the account to fund |
| lamports  | number | Yes      | Amount to request, in lamports               |
| config    | object | No       | Configuration object controlling commitment  |

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
    "method": "requestAirdrop",
    "params": ["JUP6LkbZbjS1jKKwapdHNy74zcZ3tLUZoi5QNyVTaV4", 1000000000],
    "id": "getblock.io"
}'
```
{% endcode %}
{% endtab %}

{% tab title="@solana/web3.js" %}
{% code title="example.js" %}
```javascript
const { Connection, PublicKey, LAMPORTS_PER_SOL } = require('@solana/web3.js');

const connection = new Connection('https://shared.eu-central-1.getblock.io/<ACCESS-TOKEN>/', 'confirmed');

const airdropSignature = await connection.requestAirdrop(
  new PublicKey('JUP6LkbZbjS1jKKwapdHNy74zcZ3tLUZoi5QNyVTaV4'),
  LAMPORTS_PER_SOL
);

console.log(airdropSignature);
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
        'method': 'requestAirdrop',
        'params': ["JUP6LkbZbjS1jKKwapdHNy74zcZ3tLUZoi5QNyVTaV4", 1000000000],
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
            "method": "requestAirdrop",
            "params": ["JUP6LkbZbjS1jKKwapdHNy74zcZ3tLUZoi5QNyVTaV4", 1000000000],
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

{% code overflow="wrap" %}
```json
{
    "jsonrpc": "2.0",
    "id": "getblock.io",
    "result": "5wHu1qwD4kLwYqPmy1uDCgpiJ1qGpVYU3n5aHT6bSWUE1JzcbQCPSDLDPGrFyqmLLzQjLNPTPtRZbNbUJQNMkaWq"
}
```
{% endcode %}

## Response Parameters

| Parameter | Type   | Description                                         |
| --------- | ------ | --------------------------------------------------- |
| jsonrpc   | string | JSON-RPC protocol version ("2.0")                   |
| id        | string | Request identifier matching the request             |
| result    | string | Base58-encoded signature of the airdrop transaction |

## Use Cases

* **Test Funding**: Fund a fresh keypair before running an integration test suite
* **CI Pipelines**: Top up a deployer account at the start of a Devnet workflow
* **Workshop Setup**: Seed participant wallets during a live developer session
* **Program Testing**: Fund PDAs and fee payers before invoking a deployed program

## Error Handling

| Error Code | Message          | Description                                                        |
| ---------- | ---------------- | ------------------------------------------------------------------ |
| -32602     | Invalid params   | Requested amount exceeds the faucet cap or the Pubkey is malformed |
| -32603     | Internal error   | Faucet is unavailable or rate limiting the request                 |
| -32601     | Method not found | Airdrops are disabled on Mainnet Beta                              |
