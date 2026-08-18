---
description: >-
  Example code for the getMinimumBalanceForRentExemption JSON-RPC method.
  Complete guide on how to use the getMinimumBalanceForRentExemption JSON-RPC
  method in the GetBlock Web3 documentation.
---

# getMinimumBalanceForRentExemption - Solana

This method returns the minimum lamport balance an account must hold to be exempt from rent collection at a given data size. Accounts below this threshold are subject to rent and can be purged.

## Parameters

| Parameter  | Type   | Required | Description                                 |
| ---------- | ------ | -------- | ------------------------------------------- |
| dataLength | number | Yes      | Account data length in bytes                |
| config     | object | No       | Configuration object controlling commitment |

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
    "method": "getMinimumBalanceForRentExemption",
    "params": [165],
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

const lamports = await connection.getMinimumBalanceForRentExemption(165);

console.log(lamports);
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
        'method': 'getMinimumBalanceForRentExemption',
        'params': [165],
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
            "method": "getMinimumBalanceForRentExemption",
            "params": [165],
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
    "result": 2039280
}
```

## Response Parameters

| Parameter | Type   | Description                                                               |
| --------- | ------ | ------------------------------------------------------------------------- |
| jsonrpc   | string | JSON-RPC protocol version ("2.0")                                         |
| id        | string | Request identifier matching the request                                   |
| result    | number | Minimum lamports required for rent exemption at the requested data length |

## Use Cases

* **Account Creation**: Fund a new SPL Token account with the exact rent-exempt minimum
* **Cost Estimation**: Quote storage cost to users before a program allocates state
* **PDA Sizing**: Compare rent cost across candidate account layouts
* **Reclaim Logic**: Calculate lamports recoverable when closing an account

## Error Handling

| Error Code | Message        | Description                              |
| ---------- | -------------- | ---------------------------------------- |
| -32602     | Invalid params | dataLength is negative or not an integer |
| -32603     | Internal error | Node failed to read the rent sysvar      |
