---
description: >-
  Example code for the net_listening JSON RPC method. Сomplete guide on how to
  use net_listening JSON RPC in GetBlock Web3 documentation.
---

# net\_listening - Ethereum

This method returns whether the node is actively listening for network connections from peers. On managed RPC endpoints the response is virtually always `true` — the field's diagnostic value is highest on self-hosted nodes where isolation from the P2P network indicates a configuration or connectivity issue.

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
    "method": "net_listening",
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
    method: 'net_listening',
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
        'method': 'net_listening',
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
            "method": "net_listening",
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
    "result": true
}
```

## Response Parameters

| Parameter | Type    | Description                                                                                       |
| --------- | ------- | ------------------------------------------------------------------------------------------------- |
| `jsonrpc` | string  | JSON-RPC protocol version ("2.0")                                                                 |
| `id`      | string  | Request identifier matching the request                                                           |
| `result`  | boolean | `true` if the node is listening for peers, `false` if the P2P listener is disabled or unreachable |

## Use Cases

* **Node Health Probes**: Basic reachability check as part of an endpoint availability monitor
* **Self-Hosted Node Diagnostics**: Detect P2P isolation on a self-run node (managed endpoints won't return `false` in normal operation)
* **Load Balancer Health Checks**: Simple boolean liveness probe for infrastructure-level health checks

## Error Handling

| Error Code | Message        | Description                                        |
| ---------- | -------------- | -------------------------------------------------- |
| -32603     | Internal error | Node's P2P listener status could not be determined |

## Web3 Integration

{% tabs %}
{% tab title="Ethers.js" %}
{% code title="ethers-example.js" %}
```javascript
import { ethers } from 'ethers';

const provider = new ethers.JsonRpcProvider('https://shared.eu-central-1.getblock.io/<ACCESS-TOKEN>/');

const isListening = await provider.send('net_listening', []);
console.log('Listening for peers:', isListening);
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

const isListening = await client.request({ method: 'net_listening' });
console.log('Listening for peers:', isListening);
```
{% endcode %}
{% endtab %}
{% endtabs %}
