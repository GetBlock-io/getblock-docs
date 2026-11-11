---
description: >-
  Example code for the getHealth JSON-RPC method. Complete guide on how to use
  the getHealth JSON-RPC method in the GetBlock Web3 documentation.
---

# getHealth - Solana

This method reports whether the node is caught up with the cluster tip. A healthy node returns the string `ok`; an unhealthy node returns a JSON-RPC error.

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
    "method": "getHealth",
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

const health = await connection._rpcRequest('getHealth', []);

console.log(health.result);
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
        'method': 'getHealth',
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
            "method": "getHealth",
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
    "result": "ok"
}
```

## Response Parameters

| Parameter | Type   | Description                                                                          |
| --------- | ------ | ------------------------------------------------------------------------------------ |
| jsonrpc   | string | JSON-RPC protocol version ("2.0")                                                    |
| id        | string | Request identifier matching the request                                              |
| result    | string | The string `ok` when the node is within the healthy slot distance of the cluster tip |

## Use Cases

* **Load Balancer Probes**: Drop an endpoint from rotation when it stops returning `ok`
* **Failover Logic**: Switch to a backup provider before requests start returning stale data
* **Uptime Monitoring**: Poll from an external checker to record node availability
* **Deployment Gates**: Block a release pipeline while the target node is catching up

## Error Handling

| Error Code | Message           | Description                                                                  |
| ---------- | ----------------- | ---------------------------------------------------------------------------- |
| -32005     | Node is unhealthy | The node is behind the cluster tip by more than the configured slot distance |
| -32603     | Internal error    | Node failed to evaluate its own health status                                |
