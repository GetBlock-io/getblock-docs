---
description: >-
  Example code for the web3_clientVersion JSON RPC method. Сomplete guide on how
  to use web3_clientVersion JSON RPC in GetBlock Web3 documentation.
---

# web3\_clientVersion - Ethereum

This method returns the current client version of the Ethereum execution node handling the request. Useful for detecting which client implementation (Geth, Nethermind, Besu, Reth, Erigon) is serving the endpoint and its version.

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
    "method": "web3_clientVersion",
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
    method: 'web3_clientVersion',
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
        'method': 'web3_clientVersion',
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
            "method": "web3_clientVersion",
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
    "result": "Geth/v1.14.11-stable-f3c696fa/linux-amd64/go1.23.3"
}
```

## Response Parameters

| Parameter | Type   | Description                                                                       |
| --------- | ------ | --------------------------------------------------------------------------------- |
| `jsonrpc` | string | JSON-RPC protocol version ("2.0")                                                 |
| `id`      | string | Request identifier matching the request                                           |
| `result`  | string | Client version identifier — format: `Client/vX.Y.Z-metadata/OS-arch/lang-version` |

## Use Cases

* **Client Detection**: Identify the underlying execution client to route client-specific extension calls (`debug_*`, `trace_*` availability varies by client)
* **Compatibility Checks**: Verify the node runs a version that supports a specific EIP or fork (e.g. Fusaka post-Dec 2025)
* **Node Fingerprinting**: Diagnose behavior differences across a multi-client fleet in incident response
* **Client Diversity Monitoring**: Track client-implementation distribution across endpoint fleets for decentralization audits

## Error Handling

| Error Code | Message        | Description                                                                |
| ---------- | -------------- | -------------------------------------------------------------------------- |
| -32603     | Internal error | Node failed to return its client version string (transient upstream error) |

## Web3 Integration

{% tabs %}
{% tab title="Ethers.js" %}
{% code title="ethers-example.js" %}
```javascript
import { ethers } from 'ethers';

const provider = new ethers.JsonRpcProvider('https://shared.eu-central-1.getblock.io/<ACCESS-TOKEN>/');

const clientVersion = await provider.send('web3_clientVersion', []);
console.log(clientVersion);
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

const clientVersion = await client.request({ method: 'web3_clientVersion' });
console.log(clientVersion);
```
{% endcode %}
{% endtab %}
{% endtabs %}
