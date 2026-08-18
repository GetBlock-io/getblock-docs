---
description: >-
  Example code for the getMaxRetransmitSlot JSON-RPC method. Complete guide on
  how to use the getMaxRetransmitSlot JSON-RPC method in the GetBlock Web3
  documentation.
---

# getMaxRetransmitSlot - Solana

This method returns the highest slot the node has observed through the retransmit stage of the turbine block propagation protocol.

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
    "method": "getMaxRetransmitSlot",
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

const { result } = await connection._rpcRequest('getMaxRetransmitSlot', []);

console.log(result);
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
        'method': 'getMaxRetransmitSlot',
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
            "method": "getMaxRetransmitSlot",
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
    "result": 397234561
}
```

## Response Parameters

| Parameter | Type   | Description                                      |
| --------- | ------ | ------------------------------------------------ |
| jsonrpc   | string | JSON-RPC protocol version ("2.0")                |
| id        | string | Request identifier matching the request          |
| result    | number | Highest slot seen by the node's retransmit stage |

## Use Cases

* **Propagation Lag**: Compare against getSlot to measure shred propagation delay
* **Node Diagnostics**: Detect turbine ingestion problems on a dedicated node
* **Peering Checks**: Confirm a node is receiving shreds from its upstream peers
* **Latency Benchmarks**: Compare retransmit progress across regional endpoints

## Error Handling

| Error Code | Message           | Description                                     |
| ---------- | ----------------- | ----------------------------------------------- |
| -32603     | Internal error    | Node failed to read the requested cluster state |
| -32005     | Node is unhealthy | The node has fallen behind the cluster tip      |
