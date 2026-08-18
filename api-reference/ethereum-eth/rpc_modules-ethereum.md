---
description: >-
  Example code for the rpc_modules JSON RPC method. Сomplete guide on how to use
  rpc_modules JSON RPC in GetBlock Web3 documentation.
---

# rpc\_modules - Ethereum

This method returns the list of available RPC modules exposed by the node, along with their versions. Modules are logical namespaces (`eth`, `net`, `web3`, `debug`, `trace`, `txpool`, etc.) — the returned map indicates which of them are enabled and can be called on this endpoint. Useful for feature detection.

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
    "method": "rpc_modules",
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
    method: 'rpc_modules',
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
        'method': 'rpc_modules',
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
            "method": "rpc_modules",
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
        "eth": "1.0",
        "net": "1.0",
        "web3": "1.0",
        "debug": "1.0",
        "rpc": "1.0"
    }
}
```

## Response Parameters

| Parameter | Type   | Description                                                                                                                  |
| --------- | ------ | ---------------------------------------------------------------------------------------------------------------------------- |
| `jsonrpc` | string | JSON-RPC protocol version ("2.0")                                                                                            |
| `id`      | string | Request identifier matching the request                                                                                      |
| `result`  | object | Map of module name → module version. Presence of a module indicates its methods (`<module>_*`) are callable on this endpoint |

## Use Cases

* **Feature Detection**: Detect whether `debug_*` methods are available before attempting them (avoids `method not found` errors)
* **Tier Discovery**: Distinguish shared-node endpoints (typically no `debug`/`trace` modules) from dedicated-node endpoints (full module set)
* **Client Compatibility Checks**: Verify a client-specific extension module is enabled before using it

## Error Handling

| Error Code | Message        | Description                              |
| ---------- | -------------- | ---------------------------------------- |
| -32603     | Internal error | Node failed to enumerate its RPC modules |

## Web3 Integration

{% tabs %}
{% tab title="Ethers.js" %}
{% code title="ethers-example.js" %}
```javascript
import { ethers } from 'ethers';

const provider = new ethers.JsonRpcProvider('https://shared.eu-central-1.getblock.io/<ACCESS-TOKEN>/');

const modules = await provider.send('rpc_modules', []);
console.log('Available modules:', modules);

// Feature-detect debug module before calling debug_*:
if ('debug' in modules) {
    console.log('debug_* methods available');
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

const modules = await client.request({ method: 'rpc_modules' });
console.log('Available modules:', modules);
```
{% endcode %}
{% endtab %}
{% endtabs %}
