---
description: >-
  Example code for the getStakeMinimumDelegation JSON-RPC method. Complete guide
  on how to use the getStakeMinimumDelegation JSON-RPC method in the GetBlock
  Web3 documentation.
---

# getStakeMinimumDelegation - Solana

This method returns the minimum stake delegation the cluster currently enforces, in lamports. Delegations below this threshold are rejected by the stake program.

## Parameters

| Parameter | Type   | Required | Description                                 |
| --------- | ------ | -------- | ------------------------------------------- |
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
    "method": "getStakeMinimumDelegation",
    "params": [{"commitment": "finalized"}],
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

const { value } = await connection.getStakeMinimumDelegation();

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
        'method': 'getStakeMinimumDelegation',
        'params': [{"commitment": "finalized"}],
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
            "method": "getStakeMinimumDelegation",
            "params": [{"commitment": "finalized"}],
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
        "value": 1000000000
    }
}
```

## Response Parameters

| Parameter | Type   | Description                                                           |
| --------- | ------ | --------------------------------------------------------------------- |
| jsonrpc   | string | JSON-RPC protocol version ("2.0")                                     |
| id        | string | Request identifier matching the request                               |
| result    | object | RPC response object where value is the minimum delegation in lamports |

## Use Cases

* **Stake UI Validation**: Block delegation attempts below the enforced minimum
* **Split Planning**: Confirm both halves clear the minimum before splitting a stake account
* **Liquid Staking**: Size validator allocations in a stake pool against the threshold
* **Error Prevention**: Avoid InsufficientDelegation failures before signing

## Error Handling

| Error Code | Message        | Description                                         |
| ---------- | -------------- | --------------------------------------------------- |
| -32602     | Invalid params | Unrecognized commitment level in the config object  |
| -32603     | Internal error | Node failed to read the stake program feature state |
