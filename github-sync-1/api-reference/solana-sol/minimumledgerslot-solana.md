---
description: >-
  Example code for the minimumLedgerSlot JSON-RPC method. Complete guide on how
  to use the minimumLedgerSlot JSON-RPC method in the GetBlock Web3
  documentation.
---

# minimumLedgerSlot - Solana

This method returns the lowest slot still present in the node's local ledger. The value increases over time as the node purges older slots.

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
    "method": "minimumLedgerSlot",
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

const minSlot = await connection.getFirstAvailableBlock();

console.log(minSlot);
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
        'method': 'minimumLedgerSlot',
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
            "method": "minimumLedgerSlot",
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

| Parameter | Type   | Description                                     |
| --------- | ------ | ----------------------------------------------- |
| jsonrpc   | string | JSON-RPC protocol version ("2.0")               |
| id        | string | Request identifier matching the request         |
| result    | number | Lowest slot retained in the node's local ledger |

## Use Cases

* **Retention Checks**: Confirm a node still holds the slot range an application depends on
* **Purge Monitoring**: Track how quickly ledger cleanup advances on a dedicated node
* **Query Routing**: Send requests below this slot to an archival endpoint
* **Disk Planning**: Correlate retained slot depth with node storage capacity

## Error Handling

| Error Code | Message           | Description                                 |
| ---------- | ----------------- | ------------------------------------------- |
| -32603     | Internal error    | Node failed to read the ledger cleanup slot |
| -32005     | Node is unhealthy | The node has fallen behind the cluster tip  |
