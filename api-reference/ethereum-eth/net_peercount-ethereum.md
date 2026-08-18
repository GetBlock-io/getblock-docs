---
description: >-
  Example code for the net_peerCount JSON RPC method. Сomplete guide on how to
  use net_peerCount JSON RPC in GetBlock Web3 documentation.
---

# net\_peerCount - Ethereum

This method returns the number of peers currently connected to the node's P2P layer, as a hex-encoded integer. Managed RPC endpoints commonly aggregate this across a load-balanced fleet or return the peer count of the specific backend node handling the request.

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
    "method": "net_peerCount",
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
    method: 'net_peerCount',
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
        'method': 'net_peerCount',
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
            "method": "net_peerCount",
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
    "result": "0x40"
}
```

## Response Parameters

| Parameter | Type   | Description                             |
| --------- | ------ | --------------------------------------- |
| `jsonrpc` | string | JSON-RPC protocol version ("2.0")       |
| `id`      | string | Request identifier matching the request |
| `result`  | string | Hex-encoded number of active peers      |

## Use Cases

* **Node Isolation Detection**: Detect when a node has dropped below a peer-count threshold indicating a network problem
* **Fleet Health Monitoring**: Track peer count distribution across a managed node fleet for capacity planning
* **Self-Hosted Node Diagnostics**: Diagnose connectivity issues when a node stops syncing

## Error Handling

| Error Code | Message        | Description                         |
| ---------- | -------------- | ----------------------------------- |
| -32603     | Internal error | Node's peer table could not be read |

## Web3 Integration

{% tabs %}
{% tab title="Ethers.js" %}
{% code title="ethers-example.js" %}
```javascript
import { ethers } from 'ethers';

const provider = new ethers.JsonRpcProvider('https://shared.eu-central-1.getblock.io/<ACCESS-TOKEN>/');

const peerCountHex = await provider.send('net_peerCount', []);
const peerCount = parseInt(peerCountHex, 16);
console.log('Peer count:', peerCount);
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

const peerCountHex = await client.request({ method: 'net_peerCount' });
const peerCount = parseInt(peerCountHex, 16);
console.log('Peer count:', peerCount);
```
{% endcode %}
{% endtab %}
{% endtabs %}
