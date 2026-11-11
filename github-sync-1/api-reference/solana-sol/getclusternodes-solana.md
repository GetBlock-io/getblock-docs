---
description: >-
  Example code for the getClusterNodes JSON-RPC method. Complete guide on how to
  use the getClusterNodes JSON-RPC method in the GetBlock Web3 documentation.
---

# getClusterNodes - Solana

This method returns information about all nodes the queried validator has discovered through gossip, including their network endpoints and software versions.

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
    "method": "getClusterNodes",
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

const nodes = await connection.getClusterNodes();

console.log(nodes.length, nodes[0].version);
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
        'method': 'getClusterNodes',
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
            "method": "getClusterNodes",
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
    "result": [
        {
            "featureSet": 3271946008,
            "gossip": "145.40.94.130:8001",
            "pubkey": "dv1ZAGvdsz5hHLwWXsVnM94hWf1pjbKVau1QVkaMJ92",
            "pubsub": null,
            "rpc": null,
            "shredVersion": 50093,
            "tpu": "145.40.94.130:8004",
            "tpuQuic": "145.40.94.130:8009",
            "version": "2.3.6"
        }
    ]
}
```

## Response Parameters

| Parameter | Type   | Description                                     |
| --------- | ------ | ----------------------------------------------- |
| jsonrpc   | string | JSON-RPC protocol version ("2.0")               |
| id        | string | Request identifier matching the request         |
| result    | array  | Array of node objects discovered through gossip |

### Node Entry

| Field        | Type   | Description                          |
| ------------ | ------ | ------------------------------------ |
| pubkey       | string | Node identity Pubkey, base58 encoded |
| gossip       | string | null                                 |
| tpu          | string | null                                 |
| rpc          | string | null                                 |
| version      | string | null                                 |
| featureSet   | number | null                                 |
| shredVersion | number | null                                 |

## Use Cases

* **Version Census**: Measure adoption of a validator client release across the cluster
* **Leader Targeting**: Resolve TPU addresses to forward transactions directly to leaders
* **Fork Detection**: Compare shred versions to spot nodes on a divergent cluster
* **Topology Mapping**: Chart validator distribution across hosting providers and regions

## Error Handling

| Error Code | Message           | Description                                     |
| ---------- | ----------------- | ----------------------------------------------- |
| -32603     | Internal error    | Node failed to read the requested cluster state |
| -32005     | Node is unhealthy | The node has fallen behind the cluster tip      |
