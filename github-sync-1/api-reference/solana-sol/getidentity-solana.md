---
description: >-
  Example code for the getIdentity JSON-RPC method. Complete guide on how to use
  getIdentity JSON-RPC in GetBlock Web3 documentation.
---

# getIdentity - Solana

This method returns the identity Pubkey of the node serving the request. The identity keypair signs the node's gossip messages and, for validators, its blocks.

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
    "method": "getIdentity",
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

const { result } = await connection._rpcRequest('getIdentity', []);

console.log(result.identity);
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
        'method': 'getIdentity',
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
            "method": "getIdentity",
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
        "identity": "dv1ZAGvdsz5hHLwWXsVnM94hWf1pjbKVau1QVkaMJ92"
    }
}
```

## Response Parameters

| Parameter | Type   | Description                                  |
| --------- | ------ | -------------------------------------------- |
| jsonrpc   | string | JSON-RPC protocol version ("2.0")            |
| id        | string | Request identifier matching the request      |
| result    | object | Object containing the node's identity Pubkey |

### Result Object

| Field    | Type   | Description                                           |
| -------- | ------ | ----------------------------------------------------- |
| identity | string | Base58-encoded identity Pubkey of the responding node |

## Use Cases

* **Endpoint Fingerprinting**: Identify which backend node served a request behind a load balancer
* **Sticky Debugging**: Correlate inconsistent responses with a specific node identity
* **Fleet Inventory**: Map dedicated node endpoints to their identity keys
* **Support Diagnostics**: Attach node identity to bug reports about response discrepancies

## Error Handling

| Error Code | Message           | Description                                     |
| ---------- | ----------------- | ----------------------------------------------- |
| -32603     | Internal error    | Node failed to read the requested cluster state |
| -32005     | Node is unhealthy | The node has fallen behind the cluster tip      |
