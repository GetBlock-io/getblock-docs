---
description: >-
  Example code for the getTransactionCount JSON-RPC method. Complete guide on
  how to use the getTransactionCount JSON-RPC method in the GetBlock Web3
  documentation.
---

# getTransactionCount - Solana

This method returns the total number of transactions the cluster has processed since genesis, at the requested commitment level.

## Parameters

| Parameter | Type   | Required | Description                                 |
| --------- | ------ | -------- | ------------------------------------------- |
| config    | object | No       | Configuration object controlling commitment |

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
    "method": "getTransactionCount",
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

const count = await connection.getTransactionCount();

console.log(count);
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
        'method': 'getTransactionCount',
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
            "method": "getTransactionCount",
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
    "result": 412873094521
}
```

## Response Parameters

| Parameter | Type   | Description                                |
| --------- | ------ | ------------------------------------------ |
| jsonrpc   | string | JSON-RPC protocol version ("2.0")          |
| id        | string | Request identifier matching the request    |
| result    | number | Cumulative transaction count since genesis |

## Use Cases

* **Network Metrics**: Chart cumulative throughput on a network status page
* **Sampled TPS**: Difference two readings over a known interval to estimate throughput
* **Node Comparison**: Check that two RPC nodes report consistent cluster state
* **Milestone Tracking**: Detect when the cluster crosses a transaction count threshold

## Error Handling

| Error Code | Message        | Description                                        |
| ---------- | -------------- | -------------------------------------------------- |
| -32602     | Invalid params | Unrecognized commitment level in the config object |
| -32603     | Internal error | Node failed to read the bank transaction count     |
