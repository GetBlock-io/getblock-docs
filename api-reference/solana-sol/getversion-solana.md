---
description: >-
  Example code for the getVersion JSON-RPC method. Complete guide on how to use
  the getVersion JSON-RPC method in the GetBlock Web3 documentation.
---

# getVersion - Solana

This method returns the software version running on the node, along with the unique identifier of its active feature set.

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
    "method": "behavior",
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

const version = await connection.getVersion();

console.log(version['solana-core'], version['feature-set']);
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
        'method': 'getVersion',
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
            "method": "getVersion",
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
        "feature-set": 3271946008,
        "solana-core": "2.3.6"
    }
}
```

## Response Parameters

| Parameter | Type   | Description                                                           |
| --------- | ------ | --------------------------------------------------------------------- |
| jsonrpc   | string | JSON-RPC protocol version ("2.0")                                     |
| id        | string | Request identifier matching the request                               |
| result    | object | Object holding the node's software version and feature set identifier |

### Result Object

| Field       | Type   | Description                                        |
| ----------- | ------ | -------------------------------------------------- |
| solana-core | string | Validator software version string                  |
| feature-set | number | Unique identifier of the node's active feature set |

## Use Cases

* **Compatibility Gates**: Require a minimum node version before using a newer RPC option
* **Upgrade Tracking**: Confirm an endpoint has been upgraded after a scheduled release
* **Feature Detection**: Match the feature set against activation records before relying on a feature
* **Support Reporting**: Include node version when reporting unexpected RPC behaviour

## Error Handling

| Error Code | Message           | Description                                     |
| ---------- | ----------------- | ----------------------------------------------- |
| -32603     | Internal error    | Node failed to read the requested cluster state |
| -32005     | Node is unhealthy | The node has fallen behind the cluster tip      |
