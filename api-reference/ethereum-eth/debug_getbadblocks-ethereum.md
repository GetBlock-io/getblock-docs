---
description: >-
  Example code for the debug_getBadBlocks JSON RPC method. Сomplete guide on how
  to use debug_getBadBlocks JSON RPC in GetBlock Web3 documentation.
---

# debug\_getBadBlocks - Ethereum

This method returns the list of recent blocks that the node considered invalid and rejected — usually due to invalid state transitions, invalid signatures, or protocol-rule violations. Small in normal operation; growing bad-block lists indicate a client bug, malicious block-builder behavior, or network fork-related issues.

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
    "method": "debug_getBadBlocks",
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
    method: 'debug_getBadBlocks',
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
        'method': 'debug_getBadBlocks',
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
            "method": "debug_getBadBlocks",
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
    "result": [
        {
            "hash": "0xbadb100c00000000000000000000000000000000000000000000000000000000",
            "block": {
                "number": "0x171f000",
                "hash": "0xbadb100c00000000000000000000000000000000000000000000000000000000",
                "parentHash": "0x8db7d1e5a67c7e4f1bb2d4c50a9c27c3b4a7f9e6b7a4919c2d5b0a6f3d9e1c4b",
                "timestamp": "0x67abcdef"
            },
            "rlp": "0xf90...",
            "reason": "invalid state root"
        }
    ]
}
```

## Response Parameters

| Parameter | Type            | Description                                                             |
| --------- | --------------- | ----------------------------------------------------------------------- |
| `jsonrpc` | string          | JSON-RPC protocol version ("2.0")                                       |
| `id`      | string          | Request identifier matching the request                                 |
| `result`  | array of object | Array of bad-block records with block header, RLP, and rejection reason |

## Use Cases

* **Node Health Monitoring**: Alert on non-empty bad-block lists — indicates a client bug or hostile block-builder
* **Fork Detection**: Detect network splits by comparing bad-block lists across nodes
* **Consensus Client Debugging**: Diagnose why specific blocks were rejected during consensus incidents

## Error Handling

| Error Code | Message          | Description                                               |
| ---------- | ---------------- | --------------------------------------------------------- |
| -32601     | Method not found | `debug` module not enabled — Dedicated Node tier required |
| -32603     | Internal error   | Node failed to enumerate bad-block state                  |

## Web3 Integration

{% tabs %}
{% tab title="Ethers.js" %}
{% code title="ethers-example.js" %}
```javascript
import { ethers } from 'ethers';

const provider = new ethers.JsonRpcProvider('https://shared.eu-central-1.getblock.io/<ACCESS-TOKEN>/');

const badBlocks = await provider.send('debug_getBadBlocks', []);
console.log('Bad blocks:', badBlocks.length);
if (badBlocks.length > 0) {
    console.warn('Node health issue — bad blocks present');
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

const badBlocks = await client.request({ method: 'debug_getBadBlocks' });
console.log('Bad blocks:', badBlocks.length);
```
{% endcode %}
{% endtab %}
{% endtabs %}
