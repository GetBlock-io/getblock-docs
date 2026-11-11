---
description: >-
  Example code for the debug_storageRangeAt JSON RPC method. Сomplete guide on
  how to use debug_storageRangeAt JSON RPC in GetBlock Web3 documentation.
---

# debug\_storageRangeAt - Ethereum

This method returns a range of storage slots from a specific contract at a specific block. Given a contract address and starting storage key hash, returns the next N slots along with a continuation cursor. Used for exhaustive storage-layout inspection during contract analysis, forensics, and archival research.

## Parameters

| Parameter    | Type    | Required | Description                                               |
| ------------ | ------- | -------- | --------------------------------------------------------- |
| `blockHash`  | string  | Yes      | 32-byte block hash                                        |
| `txIndex`    | integer | Yes      | Transaction index within the block to snapshot storage at |
| `address`    | string  | Yes      | 20-byte contract address                                  |
| `startKey`   | string  | Yes      | Hex-encoded starting storage-key hash                     |
| `maxResults` | integer | Yes      | Maximum number of slots to return in this page            |

## Request

{% tabs %}
{% tab title="cURL" %}
{% code overflow="wrap" %}
```bash
curl --location --request POST 'https://shared.eu-central-1.getblock.io/<ACCESS-TOKEN>/' \
--header 'Content-Type: application/json' \
--data-raw '{
    "jsonrpc": "2.0",
    "method": "debug_storageRangeAt",
    "params": [
        "0x9ec8e2f6b78d8f5f2ac3e5d61b0d38d4d5a8f0e7c8b5a2f9d3e6c1b7a4d0f2e5c",
        0,
        "0xC02aaA39b223FE8D0A0e5C4F27eAD9083C756Cc2",
        "0x0000000000000000000000000000000000000000000000000000000000000000",
        10
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
    method: 'debug_storageRangeAt',
    params: [
        "0x9ec8e2f6b78d8f5f2ac3e5d61b0d38d4d5a8f0e7c8b5a2f9d3e6c1b7a4d0f2e5c",
        0,
        "0xC02aaA39b223FE8D0A0e5C4F27eAD9083C756Cc2",
        "0x0000000000000000000000000000000000000000000000000000000000000000",
        10
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
        'method': 'debug_storageRangeAt',
        'params': [
        "0x9ec8e2f6b78d8f5f2ac3e5d61b0d38d4d5a8f0e7c8b5a2f9d3e6c1b7a4d0f2e5c",
        0,
        "0xC02aaA39b223FE8D0A0e5C4F27eAD9083C756Cc2",
        "0x0000000000000000000000000000000000000000000000000000000000000000",
        10
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
            "method": "debug_storageRangeAt",
            "params": [
        "0x9ec8e2f6b78d8f5f2ac3e5d61b0d38d4d5a8f0e7c8b5a2f9d3e6c1b7a4d0f2e5c",
        0,
        "0xC02aaA39b223FE8D0A0e5C4F27eAD9083C756Cc2",
        "0x0000000000000000000000000000000000000000000000000000000000000000",
        10
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
    "result": {
        "storage": {
            "0x290decd9548b62a8d60345a988386fc84ba6bc95484008f6362f93160ef3e563": {
                "key": "0x0000000000000000000000000000000000000000000000000000000000000000",
                "value": "0x000000000000000000000000000000000000000000000000000000000000000a"
            }
        },
        "nextKey": "0xb10e2d527612073b26eecdfd717e6a320cf44b4afac2b0732d9fcbe2b7fa0cf6"
    }
}
```

## Response Parameters

| Parameter        | Type   | Description                                                              |
| ---------------- | ------ | ------------------------------------------------------------------------ |
| `jsonrpc`        | string | JSON-RPC protocol version ("2.0")                                        |
| `id`             | string | Request identifier matching the request                                  |
| `result.storage` | object | Map of storage-key-hash → `{key, value}` entries                         |
| `result.nextKey` | string | Continuation cursor for the next page (null when the range is exhausted) |

## Use Cases

* **Exhaustive Storage Inspection**: Enumerate all storage slots of a contract for reverse-engineering unknown storage layouts
* **Forensic Analysis**: Snapshot a contract's complete storage at the time of an exploit for post-mortem
* **Mapping Enumeration**: Iterate through all keys of a Solidity `mapping` when the key set isn't known in advance

## Error Handling

| Error Code | Message          | Description                                                                             |
| ---------- | ---------------- | --------------------------------------------------------------------------------------- |
| -32601     | Method not found | `debug` module not enabled — Dedicated Node tier required                               |
| -32602     | Invalid params   | Block hash, address, or start key is malformed                                          |
| -32603     | Internal error   | Node failed to enumerate storage (often indicates state pruning at the requested block) |

## Web3 Integration

{% tabs %}
{% tab title="Ethers.js" %}
{% code title="ethers-example.js" %}
```javascript
import { ethers } from 'ethers';

const provider = new ethers.JsonRpcProvider('https://shared.eu-central-1.getblock.io/<ACCESS-TOKEN>/');

const range = await provider.send('debug_storageRangeAt', [
    '0x9ec8e2f6b78d8f5f2ac3e5d61b0d38d4d5a8f0e7c8b5a2f9d3e6c1b7a4d0f2e5c',
    0,
    '0xC02aaA39b223FE8D0A0e5C4F27eAD9083C756Cc2',
    '0x0000000000000000000000000000000000000000000000000000000000000000',
    10
]);
console.log('Slots returned:', Object.keys(range.storage).length);
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

const range = await client.request({
    method: 'debug_storageRangeAt',
    params: [
        '0x9ec8e2f6b78d8f5f2ac3e5d61b0d38d4d5a8f0e7c8b5a2f9d3e6c1b7a4d0f2e5c', 0, '0xC02aaA39b223FE8D0A0e5C4F27eAD9083C756Cc2',
        '0x' + '0'.repeat(64), 10
    ],
});
console.log('Slots:', Object.keys(range.storage).length);
```
{% endcode %}
{% endtab %}
{% endtabs %}
