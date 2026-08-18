---
description: >-
  Example code for the eth_syncing JSON-RPC method. Complete guide to using the
  eth_syncing JSON-RPC method in the GetBlock.io Web3 documentation.
---

# eth\_syncing - Unchain

This method returns the sync status of the node. It returns false when the node is fully synced, or an object describing sync progress while it is catching up.

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
    "method": "eth_syncing",
    "params": [],
    "id": "getblock.io"
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
    method: 'eth_syncing',
    params: [],
    id: 'getblock.io'
}, {
    headers: { 'Content-Type': 'application/json' }
});

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
        'method': 'eth_syncing',
        'params': [],
        'id': 'getblock.io'
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
        .header("Content-Type", "application/json")
        .json(&json!({
            "jsonrpc": "2.0",
            "method": "eth_syncing",
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
    "result": false
}
```

## Response Parameters

| Parameter | Type              | Description                                                    |
| --------- | ----------------- | -------------------------------------------------------------- |
| jsonrpc   | string            | JSON-RPC protocol version ("2.0")                              |
| id        | string            | Request identifier matching the request                        |
| result    | boolean or object | false when synced, or a sync-progress object while catching up |

## Use Cases

* **Readiness Checks**: Confirm a node is synced before serving traffic
* **Sync Monitoring**: Track a node's catch-up progress
* **Load Balancing**: Route requests away from unsynced nodes
* **Diagnostics**: Detect a node that has fallen behind the chain

## Error Handling

| Error Code | Message        | Description                                            |
| ---------- | -------------- | ------------------------------------------------------ |
| -32602     | Invalid params | A parameter is missing or has the wrong type or format |
| -32603     | Internal error | The node failed to process the request                 |

## Web3 Integration

{% tabs %}
{% tab title="Ethers.js" %}
{% code title="ethers-example.js" %}
```javascript
const { ethers } = require('ethers');

const provider = new ethers.JsonRpcProvider('https://shared.eu-central-1.getblock.io/<ACCESS-TOKEN>/');

const syncing = await provider.send('eth_syncing', []);
console.log(syncing);
```
{% endcode %}
{% endtab %}

{% tab title="Viem" %}
{% code title="viem-example.js" %}
```javascript
import { createPublicClient, http } from 'viem';
import { unichain } from 'viem/chains';

const client = createPublicClient({ chain: unichain, transport: http('https://shared.eu-central-1.getblock.io/<ACCESS-TOKEN>/') });

const syncing = await client.request({ method: 'eth_syncing' });
console.log(syncing);
```
{% endcode %}
{% endtab %}
{% endtabs %}
