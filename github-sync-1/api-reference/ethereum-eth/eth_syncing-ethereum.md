---
description: >-
  Example code for the eth_syncing JSON RPC method. Сomplete guide on how to use
  eth_syncing JSON RPC in GetBlock Web3 documentation.
---

# eth\_syncing - Ethereum

This method returns an object with sync status data when the node is catching up to the chain tip, or `false` when the node is fully synced. Managed RPC endpoints typically return `false` unless a specific backend node is currently mid-sync.

## Parameters

This method takes no parameters.

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

| Parameter | Type              | Description                                                                                                     |
| --------- | ----------------- | --------------------------------------------------------------------------------------------------------------- |
| `jsonrpc` | string            | JSON-RPC protocol version ("2.0")                                                                               |
| `id`      | string            | Request identifier matching the request                                                                         |
| `result`  | boolean or object | `false` if fully synced, or an object with `startingBlock`, `currentBlock`, `highestBlock` (hex) if catching up |

## Use Cases

* **Node Freshness Verification**: Confirm a node is at chain tip before treating its reads as canonical
* **Sync Progress Monitoring**: Track a self-hosted node's catch-up progress via the `currentBlock` / `highestBlock` fields
* **Failover Decisioning**: Skip a syncing backend and route to a fully-synced one at the load balancer level

## Error Handling

| Error Code | Message        | Description                             |
| ---------- | -------------- | --------------------------------------- |
| -32603     | Internal error | Node failed to determine its sync state |

## Web3 Integration

{% tabs %}
{% tab title="Ethers.js" %}
{% code title="ethers-example.js" %}
```javascript
import { ethers } from 'ethers';

const provider = new ethers.JsonRpcProvider('https://shared.eu-central-1.getblock.io/<ACCESS-TOKEN>/');

const syncStatus = await provider.send('eth_syncing', []);
if (syncStatus === false) {
    console.log('Node is fully synced');
} else {
    console.log('Syncing:', syncStatus);
}
```
{% endcode %}
{% endtab %}

{% tab title="Viem" %}
{% code title="viem-example.js" %}
```javascript
import { createPublicClient, http } from 'viem';
import { mainnet } from 'viem/chains';

const client = createPublicClient({
    chain: mainnet,
    transport: http('https://shared.eu-central-1.getblock.io/<ACCESS-TOKEN>/'),
});

const syncStatus = await client.request({ method: 'eth_syncing' });
if (syncStatus === false) {
    console.log('Node is fully synced');
} else {
    console.log('Syncing:', syncStatus);
}
```
{% endcode %}
{% endtab %}
{% endtabs %}
