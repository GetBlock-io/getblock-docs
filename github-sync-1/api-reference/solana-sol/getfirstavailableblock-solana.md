---
description: >-
  Example code for the getFirstAvailableBlock JSON-RPC method. Complete guide on
  how to use the getFirstAvailableBlock JSON-RPC method in the GetBlock Web3
  documentation.
---

# getFirstAvailableBlock - Solana

This method returns the lowest confirmed block slot that has not been purged from the node's ledger. It defines the earliest point historical queries can reach on that node.

## Parameters

This method does not accept any parameters.

## Request

{% tabs %}
{% tab title="cURL" %}
{% code overflow="wrap" %}
```bash
curl --location --request POST 'https://shared.eu-central-1.getblock.io/<ACCESS-TOKEN>/' \
--header 'Content-Type: application/json' \
--data-raw '{
    "jsonrpc": "2.0",
    "method": "getFirstAvailableBlock",
    "params": [],
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

const firstSlot = await connection.getFirstAvailableBlock();

console.log(firstSlot);
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
        'method': 'getFirstAvailableBlock',
        'params': [],
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
            "method": "getFirstAvailableBlock",
            "params": [],
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
    "result": 386201344
}
```

## Response Parameters

| Parameter | Type   | Description                                            |
| --------- | ------ | ------------------------------------------------------ |
| jsonrpc   | string | JSON-RPC protocol version ("2.0")                      |
| id        | string | Request identifier matching the request                |
| result    | number | Lowest confirmed block slot still retained by the node |

## Use Cases

* **History Bounds**: Check whether a target slot is inside the node's retained range
* **Archive Routing**: Route older queries to an archive endpoint when the slot falls below this value
* **Backfill Planning**: Set the floor for a historical ingestion job
* **Error Prevention**: Avoid slot-missing errors by validating range inputs up front

## Error Handling

| Error Code | Message           | Description                                                 |
| ---------- | ----------------- | ----------------------------------------------------------- |
| -32603     | Internal error    | Node failed to read the lowest cleanup slot from blockstore |
| -32005     | Node is unhealthy | The node has fallen behind the cluster tip                  |
