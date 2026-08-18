---
description: >-
  Example code for the getHighestSnapshotSlot JSON-RPC method. Complete guide on
  how to use the getHighestSnapshotSlot JSON-RPC method in the GetBlock Web3
  documentation.
---

# getHighestSnapshotSlot - Solana

This method returns the highest slot for which the node has a full snapshot, and the highest incremental snapshot slot built on top of it when one exists.

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
    "method": "getHighestSnapshotSlot",
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

const snapshot = await connection._rpcRequest('getHighestSnapshotSlot', []);

console.log(snapshot.result);
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
        'method': 'getHighestSnapshotSlot',
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
            "method": "getHighestSnapshotSlot",
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
    "result": {
        "full": 397200000,
        "incremental": 397233361
    }
}
```

## Response Parameters

| Parameter | Type   | Description                                                    |
| --------- | ------ | -------------------------------------------------------------- |
| jsonrpc   | string | JSON-RPC protocol version ("2.0")                              |
| id        | string | Request identifier matching the request                        |
| result    | object | Object holding the highest full and incremental snapshot slots |

### Result Object

| Field       | Type   | Description                       |
| ----------- | ------ | --------------------------------- |
| full        | number | Highest slot with a full snapshot |
| incremental | number | null                              |

## Use Cases

* **Bootstrap Sources**: Pick a peer with a recent snapshot when starting a new validator
* **Snapshot Freshness**: Alert when snapshot generation falls behind schedule
* **Restart Planning**: Estimate catch-up time from the gap between snapshot and tip
* **Infrastructure Audits**: Confirm incremental snapshots are enabled on a node

## Error Handling

| Error Code | Message           | Description                                     |
| ---------- | ----------------- | ----------------------------------------------- |
| -32008     | No snapshot       | The node has not yet generated a full snapshot  |
| -32603     | Internal error    | Node failed to read the requested cluster state |
| -32005     | Node is unhealthy | The node has fallen behind the cluster tip      |
