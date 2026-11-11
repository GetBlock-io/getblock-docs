---
description: >-
  Example code for the status JSON-RPC method. Complete guide on how to use
  status JSON-RPC in GetBlock Web3 documentation.
---

# status - Axelar

Returns the node's status: latest block height and time, sync state, and validator info. The primary way to read the chain tip and confirm the node is synced.

## Parameters

{% hint style="info" %}
This method takes an empty `params` object.
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
const response = await axios.post('https://shared.eu-central-1.getblock.io/<ACCESS-TOKEN>/', { jsonrpc: '2.0', id: 'getblock.io', method: 'status', params: {} }, { headers: { 'Content-Type': 'application/json' } });
console.log(response.data.result);
```
{% endcode %}
{% endtab %}

{% tab title="Request" %}
{% code title="example.py" %}
```python
import requests
response = requests.post('https://shared.eu-central-1.getblock.io/<ACCESS-TOKEN>/', headers={'Content-Type': 'application/json'}, json={'jsonrpc': '2.0', 'id': 'getblock.io', 'method': 'status', 'params': {}})
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
    let res = client.post("https://shared.eu-central-1.getblock.io/<ACCESS-TOKEN>/").json(&json!({"jsonrpc":"2.0","id":"getblock.io","method":"status","params":{}})).send().await?.json::<Value>().await?;
    println!("{}", res["result"]);
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
            "network": "axelar-dojo-1",
            "version": "0.38.0"
        },
        "sync_info": {
            "latest_block_height": "19500000",
            "latest_block_time": "2025-11-01T12:00:00Z",
            "catching_up": false
        },
        "validator_info": {
            "voting_power": "0"
        }
    }
}
```

## Response Fields

| Field           | Type   | Description                              |
| --------------- | ------ | ---------------------------------------- |
| node\_info      | object | Network id and node version              |
| sync\_info      | object | Latest height/time and catching\_up flag |
| validator\_info | object | This node's validator info               |

## Use Cases

* **Chain Tip**: Read sync\_info.latest\_block\_height
* **Sync Detection**: Check catching\_up
* **Network Guard**: Confirm the network is axelar-dojo-1

## Error Handling

| Error                     | Message        | Description                                       |
| ------------------------- | -------------- | ------------------------------------------------- |
| -32603 / Internal error   | Internal error | The node failed to return status                  |
| 403 / RBAC: access denied | Access denied  | The GetBlock access token is missing or incorrect |
