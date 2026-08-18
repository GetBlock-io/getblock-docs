---
description: >-
  Example code for the eth_getStorageAt JSON RPC method. Сomplete guide on how
  to use eth_getStorageAt JSON RPC in GetBlock Web3 documentation.
---

# eth\_getStorageAt - Ethereum

This method returns the value at a specific storage slot of a contract at a given block, hex-encoded. Storage slots are 32-byte keys that map to 32-byte values in the contract's state trie. Reading arbitrary slots lets applications inspect contract state directly without a public getter — useful for proxy inspection, storage-layout debugging, and reading private fields.

## Parameters

| Parameter        | Type   | Required | Description                                                                  |
| ---------------- | ------ | -------- | ---------------------------------------------------------------------------- |
| `address`        | string | Yes      | Contract address (hex-encoded with `0x` prefix)                              |
| `storageSlot`    | string | Yes      | Storage slot key (hex-encoded 32-byte value)                                 |
| `blockParameter` | string | Yes      | Block number in hex, or `latest`, `earliest`, `pending`, `finalized`, `safe` |

## Request

{% tabs %}
{% tab title="cURL" %}
{% code overflow="wrap" %}
```bash
curl --location --request POST 'https://shared.eu-central-1.getblock.io/<ACCESS-TOKEN>/' \
--header 'Content-Type: application/json' \
--data-raw '{
    "jsonrpc": "2.0",
    "method": "eth_getStorageAt",
    "params": [
        "0xC02aaA39b223FE8D0A0e5C4F27eAD9083C756Cc2",
        "0x0000000000000000000000000000000000000000000000000000000000000000",
        "latest"
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
    method: 'eth_getStorageAt',
    params: [
        "0xC02aaA39b223FE8D0A0e5C4F27eAD9083C756Cc2",
        "0x0000000000000000000000000000000000000000000000000000000000000000",
        "latest"
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
        'method': 'eth_getStorageAt',
        'params': [
        "0xC02aaA39b223FE8D0A0e5C4F27eAD9083C756Cc2",
        "0x0000000000000000000000000000000000000000000000000000000000000000",
        "latest"
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
            "method": "eth_getStorageAt",
            "params": [
        "0xC02aaA39b223FE8D0A0e5C4F27eAD9083C756Cc2",
        "0x0000000000000000000000000000000000000000000000000000000000000000",
        "latest"
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
    "result": "0x000000000000000000000000000000000000000000000000000000000000000a"
}
```

## Response Parameters

| Parameter | Type   | Description                                                                                     |
| --------- | ------ | ----------------------------------------------------------------------------------------------- |
| `jsonrpc` | string | JSON-RPC protocol version ("2.0")                                                               |
| `id`      | string | Request identifier matching the request                                                         |
| `result`  | string | Hex-encoded 32-byte value at the requested slot (zero-padded if the underlying type is smaller) |

## Use Cases

* **Proxy Implementation Detection**: Read the EIP-1967 implementation slot (`0x360894a13ba1a3210667c828492db98dca3e2076cc3735a920a3ca505d382bbc`) to identify the current logic contract behind a proxy
* **Storage-Layout Debugging**: Inspect specific state variables during development when public getters are unavailable
* **Private Field Reading**: Read private state variables whose slots are known from the contract's storage layout
* **Cross-Chain Storage Proofs**: Combined with `eth_getProof`, verify storage values via Merkle proofs on other chains

## Error Handling

| Error Code | Message        | Description                                                         |
| ---------- | -------------- | ------------------------------------------------------------------- |
| -32602     | Invalid params | Address or storage slot is malformed, or block parameter is invalid |
| -32603     | Internal error | Node failed to look up storage at the requested block               |

## Web3 Integration

{% tabs %}
{% tab title="Ethers.js" %}
{% code title="ethers-example.js" %}
```javascript
import { ethers } from 'ethers';

const provider = new ethers.JsonRpcProvider('https://shared.eu-central-1.getblock.io/<ACCESS-TOKEN>/');

// Read WETH storage slot 0 (the totalSupply-adjacent slot in the WETH9 layout)
const value = await provider.getStorage('0xC02aaA39b223FE8D0A0e5C4F27eAD9083C756Cc2', 0);
console.log('Storage value:', value);
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

const value = await client.getStorageAt({
    address: '0xC02aaA39b223FE8D0A0e5C4F27eAD9083C756Cc2',
    slot: '0x0',
});
console.log('Storage value:', value);
```
{% endcode %}
{% endtab %}
{% endtabs %}
