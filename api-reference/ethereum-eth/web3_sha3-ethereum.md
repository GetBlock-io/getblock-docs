---
description: >-
  Example code for the web3_sha3 JSON RPC method. Сomplete guide on how to use
  web3_sha3 JSON RPC in GetBlock Web3 documentation.
---

# web3\_sha3 - Ethereum

This method returns the Keccak-256 (SHA3-256) hash of the given hex-encoded data. Commonly used to compute event signature topics, function selectors, and content-addressable identifiers off-chain in a way that's guaranteed to match on-chain behavior.

## Parameters

| Parameter | Type   | Required | Description                                 |
| --------- | ------ | -------- | ------------------------------------------- |
| `data`    | string | Yes      | Hex-encoded data to hash (with `0x` prefix) |

## Request

{% tabs %}
{% tab title="cURL" %}
{% code overflow="wrap" %}
```bash
curl --location --request POST 'https://shared.eu-central-1.getblock.io/<ACCESS-TOKEN>/' \
--header 'Content-Type: application/json' \
--data-raw '{
    "jsonrpc": "2.0",
    "method": "web3_sha3",
    "params": [
        "0x68656c6c6f20776f726c64"
    ],
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
    method: 'web3_sha3',
    params: [
        "0x68656c6c6f20776f726c64"
    ],
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
        'method': 'web3_sha3',
        'params': [
        "0x68656c6c6f20776f726c64"
    ],
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
            "method": "web3_sha3",
            "params": [
        "0x68656c6c6f20776f726c64"
    ],
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
    "result": "0x47173285a8d7341e5e972fc677286384f802f8ef42a5ec5f03bbfa254cb01fad"
}
```

## Response Parameters

| Parameter | Type   | Description                                               |
| --------- | ------ | --------------------------------------------------------- |
| `jsonrpc` | string | JSON-RPC protocol version ("2.0")                         |
| `id`      | string | Request identifier matching the request                   |
| `result`  | string | Keccak-256 hash of the input data, hex-encoded (32 bytes) |

## Use Cases

* **Event Signature Hashing**: Compute the `keccak256` hash of an event signature (e.g. `Transfer(address,address,uint256)`) to derive its topic0 for log filtering
* **Function Selector Derivation**: Compute the 4-byte function selector from a canonical signature for ABI encoding
* **ENS Name Hashing**: Hash ENS labels as part of computing an ENS namehash off-chain
* **Content Addressing**: Generate deterministic hashes for use as identifiers in IPFS-adjacent workflows or Merkle tree construction

## Error Handling

| Error Code | Message        | Description                              |
| ---------- | -------------- | ---------------------------------------- |
| -32602     | Invalid params | Input is not a valid hex-encoded string  |
| -32603     | Internal error | Node hashing subsystem returned an error |

## Web3 Integration

{% tabs %}
{% tab title="Ethers.js" %}
{% code title="ethers-example.js" %}
```javascript
import { ethers } from 'ethers';

// Ethers.js v6 exposes keccak256 directly — no RPC round-trip needed for
// off-chain hashing. This is the recommended path.
const hash = ethers.keccak256('0x68656c6c6f20776f726c64');
console.log(hash);

// For RPC-parity verification against the node:
const provider = new ethers.JsonRpcProvider('https://shared.eu-central-1.getblock.io/<ACCESS-TOKEN>/');
const nodeHash = await provider.send('web3_sha3', ['0x68656c6c6f20776f726c64']);
console.log(nodeHash);
```
{% endcode %}
{% endtab %}

{% tab title="Viem" %}
{% code title="viem-example.js" %}
```javascript
import { createPublicClient, http, keccak256 } from 'viem';
import { mainnet } from 'viem/chains';

// Viem exposes keccak256 directly — no RPC round-trip needed.
const hash = keccak256('0x68656c6c6f20776f726c64');
console.log(hash);

// For RPC-parity verification against the node:
const client = createPublicClient({
    chain: mainnet,
    transport: http('https://shared.eu-central-1.getblock.io/<ACCESS-TOKEN>/'),
});

const nodeHash = await client.request({
    method: 'web3_sha3',
    params: ['0x68656c6c6f20776f726c64'],
});
console.log(nodeHash);
```
{% endcode %}
{% endtab %}
{% endtabs %}
