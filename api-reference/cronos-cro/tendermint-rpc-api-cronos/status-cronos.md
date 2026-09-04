---
description: >-
  Example code for the status JSON-RPC method. Complete guide on how to use
  status JSON-RPC in GetBlock Web3 documentation.
---

# status - Cronos

Returns the current node information, sync status (latest block height and time), and the validator info for the node. Used to confirm connectivity and the chain tip.

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
    "method": "status",
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
    method: 'status',
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
        'method': 'status',
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
            "method": "status",
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
        "node_info": {
            "network": "cronosmainnet_25-1",
            "version": "0.37.5"
        },
        "sync_info": {
            "latest_block_hash": "E1F2...",
            "latest_block_height": "12345678",
            "latest_block_time": "2025-11-01T12:00:00Z",
            "catching_up": false
        },
        "validator_info": {
            "address": "F00D...",
            "voting_power": "0"
        }
    }
}
```

## Response Fields

| Field           | Type   | Description                                         |
| --------------- | ------ | --------------------------------------------------- |
| node\_info      | object | Node network id and software version                |
| sync\_info      | object | Latest block height/hash/time and catching\_up flag |
| validator\_info | object | This node's validator address and voting power      |

## Use Cases

* **Chain Tip**: Read sync\_info.latest\_block\_height as the current height
* **Sync Detection**: Detect a lagging node via sync\_info.catching\_up
* **Network Guard**: Confirm node\_info.network matches the expected chain id

## Error Handling

| Error                     | Message        | Description                                       |
| ------------------------- | -------------- | ------------------------------------------------- |
| -32603 / Internal error   | Internal error | The node failed to return status                  |
| 403 / RBAC: access denied | Access denied  | The GetBlock access token is missing or incorrect |
