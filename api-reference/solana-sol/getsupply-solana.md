---
description: >-
  Example code for the getSupply JSON-RPC method. Complete guide on how to use
  the getSupply JSON-RPC method in the GetBlock Web3 documentation.
---

# getSupply - Solana

This method returns total, circulating, and non-circulating SOL supply in lamports. The list of non-circulating accounts can be omitted to reduce response size.

## Parameters

| Parameter | Type   | Required | Description                                                     |
| --------- | ------ | -------- | --------------------------------------------------------------- |
| config    | object | No       | Configuration object controlling commitment and account listing |

### Config Object

| Field                             | Type    | Required | Description                                                                 |
| --------------------------------- | ------- | -------- | --------------------------------------------------------------------------- |
| commitment                        | string  | No       | Commitment level: processed, confirmed, or finalized. Defaults to finalized |
| excludeNonCirculatingAccountsList | boolean | No       | Omit the array of non-circulating account addresses from the response       |

## Request

{% tabs %}
{% tab title="cURL" %}
{% code overflow="wrap" %}
```bash
curl --location --request POST 'https://shared.eu-central-1.getblock.io/<ACCESS-TOKEN>/' \
--header 'Content-Type: application/json' \
--data-raw '{
    "jsonrpc": "2.0",
    "method": "getSupply",
    "params": [{"commitment": "finalized", "excludeNonCirculatingAccountsList": true}],
    "id": "getblock.io"
}'
```
{% endcode %}
{% endtab %}

{% tab title="@solana/web3.js" %}
{% code title="example.js" %}
```javascript
const { Connection } = require('@solana/web3.js');

const connection = new Connection('https://shared.eu-central-1.getblock.io/<ACCESS-TOKEN>/', 'finalized');

const { value } = await connection.getSupply({
  excludeNonCirculatingAccountsList: true
});

console.log(value.circulating, value.total);
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
        'method': 'getSupply',
        'params': [{"commitment": "finalized", "excludeNonCirculatingAccountsList": true}],
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
            "method": "getSupply",
            "params": [{"commitment": "finalized", "excludeNonCirculatingAccountsList": true}],
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
            "slot": 397234561
        },
        "value": {
            "circulating": 542837291043872913,
            "nonCirculating": 18402937182937,
            "nonCirculatingAccounts": [],
            "total": 542855693981055850
        }
    }
}
```

## Response Parameters

| Parameter | Type   | Description                                                            |
| --------- | ------ | ---------------------------------------------------------------------- |
| jsonrpc   | string | JSON-RPC protocol version ("2.0")                                      |
| id        | string | Request identifier matching the request                                |
| result    | object | RPC response object where value holds the supply breakdown in lamports |

### Value Object

| Field                  | Type   | Description                                                   |
| ---------------------- | ------ | ------------------------------------------------------------- |
| total                  | number | Total SOL supply in lamports                                  |
| circulating            | number | Circulating supply in lamports                                |
| nonCirculating         | number | Non-circulating supply in lamports                            |
| nonCirculatingAccounts | array  | Addresses holding non-circulating supply, empty when excluded |

## Use Cases

* **Market Cap**: Multiply circulating supply by SOL price for market capitalization
* **Staking Ratio**: Compare total active stake against circulating supply
* **Vesting Analysis**: Track how non-circulating balances unlock over time
* **Network Reports**: Publish verified supply figures rather than third-party estimates

## Error Handling

| Error Code | Message           | Description                                        |
| ---------- | ----------------- | -------------------------------------------------- |
| -32602     | Invalid params    | Unrecognized commitment level in the config object |
| -32603     | Internal error    | Node failed to scan non-circulating accounts       |
| -32005     | Node is unhealthy | The node has fallen behind the cluster tip         |
