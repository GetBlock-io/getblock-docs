---
description: >-
  Example code for the net_info JSON-RPC method. Complete guide on how to use
  net_info JSON-RPC in GetBlock Web3 documentation.
---

# net\_info - Cronos

Returns the node's current peer connections and listening status, including per-peer network address and node id.

## Parameters

{% hint style="info" %}
This method takes no parameters; send an empty `params` object.
{% endhint %}

## Request

{% tabs %}
{% tab title="cURL" %}
{% code overflow="wrap" %}
```bash
curl --location --request POST 'https://shared.eu-central-1.getblock.io/<ACCESS-TOKEN>/' \
--header 'Content-Type: application/json' \
--data-raw '{
    "jsonrpc": "2.0",
    "id": "getblock.io",
    "method": "net_info",
    "params": {}
}'
```
{% endcode %}
{% endtab %}

{% tab title="Axios" %}
{% code title="example.js" %}
```javascript
const axios = require('axios');

const response = await axios.post('https://shared.eu-central-1.getblock.io/<ACCESS-TOKEN>/', {
    jsonrpc: '2.0',
    id: 'getblock.io',
    method: 'net_info',
    params: {}
}, { headers: { 'Content-Type': 'application/json' } });

console.log(response.data.result);
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
        'id': 'getblock.io',
        'method': 'net_info',
        'params': {}
    }
)

print(response.json())
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
        .json(&json!({
            "jsonrpc": "2.0",
            "id": "getblock.io",
            "method": "net_info",
            "params": {}
        }))
        .send().await?
        .json::<Value>().await?;
    println!("{}", response["result"]);
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
        "listening": true,
        "listeners": [
            "Listener(@)"
        ],
        "n_peers": "8",
        "peers": [
            {
                "node_id": "abc123",
                "is_outbound": true,
                "remote_ip": "35.1.2.3"
            }
        ]
    }
}
```

## Response Fields

| Field     | Type    | Description                                       |
| --------- | ------- | ------------------------------------------------- |
| n\_peers  | string  | Number of connected peers                         |
| peers     | array   | Connected peers with node id and remote address   |
| listening | boolean | Whether the node is accepting inbound connections |

## Use Cases

* **Connectivity Health**: Detect an under-connected node via n\_peers
* **Topology**: Enumerate peer node ids and addresses
* **Monitoring**: Alert when peer count drops

## Error Handling

| Error                     | Message        | Description                                       |
| ------------------------- | -------------- | ------------------------------------------------- |
| -32603 / Internal error   | Internal error | The node failed to report peers                   |
| 403 / RBAC: access denied | Access denied  | The GetBlock access token is missing or incorrect |
