---
description: >-
  Example code for the debug_batchSendRawTransaction JSON RPC method. Сomplete
  guide on how to use debug_batchSendRawTransaction JSON RPC in GetBlock Web3
  documentation.
---

# debug\_batchSendRawTransaction - Ethereum

This method submits an array of signed raw transactions to the network in a single call. Higher throughput than N separate `eth_sendRawTransaction` calls — the node batches mempool insertion. Returns an array of results per transaction. Useful for high-frequency workflows and MEV searchers submitting multiple related transactions.

## Parameters

| Parameter            | Type            | Required | Description                                             |
| -------------------- | --------------- | -------- | ------------------------------------------------------- |
| `signedTransactions` | array of string | Yes      | Array of hex-encoded RLP-serialized signed transactions |

## Request

{% tabs %}
{% tab title="cURL" %}
{% code overflow="wrap" %}
```bash
curl --location --request POST 'https://shared.eu-central-1.getblock.io/<ACCESS-TOKEN>/' \
--header 'Content-Type: application/json' \
--data-raw '{
    "jsonrpc": "2.0",
    "method": "debug_batchSendRawTransaction",
    "params": [
        [
            "0x02f8710182046282520894c02aaa39b223fe8d0a0e5c4f27ead9083c756cc287038d7ea4c6800080c001a01c8a4bb2b9c8b0c1d2e3f4a5b6c7d8e9f0a1b2c3d4e5f6a7b8c9d0e1f2a3b4c5a04d5e6f7a8b9c0d1e2f3a4b5c6d7e8f9a0b1c2d3e4f5a6b7c8d9e0f1a2b3c4d5e6",
            "0x02f8710182046382520894c02aaa39b223fe8d0a0e5c4f27ead9083c756cc287038d7ea4c6800080c001a02c8a4bb2b9c8b0c1d2e3f4a5b6c7d8e9f0a1b2c3d4e5f6a7b8c9d0e1f2a3b4c5a05d5e6f7a8b9c0d1e2f3a4b5c6d7e8f9a0b1c2d3e4f5a6b7c8d9e0f1a2b3c4d5e6"
        ]
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
    method: 'debug_batchSendRawTransaction',
    params: [
        [
            "0x02f8710182046282520894c02aaa39b223fe8d0a0e5c4f27ead9083c756cc287038d7ea4c6800080c001a01c8a4bb2b9c8b0c1d2e3f4a5b6c7d8e9f0a1b2c3d4e5f6a7b8c9d0e1f2a3b4c5a04d5e6f7a8b9c0d1e2f3a4b5c6d7e8f9a0b1c2d3e4f5a6b7c8d9e0f1a2b3c4d5e6",
            "0x02f8710182046382520894c02aaa39b223fe8d0a0e5c4f27ead9083c756cc287038d7ea4c6800080c001a02c8a4bb2b9c8b0c1d2e3f4a5b6c7d8e9f0a1b2c3d4e5f6a7b8c9d0e1f2a3b4c5a05d5e6f7a8b9c0d1e2f3a4b5c6d7e8f9a0b1c2d3e4f5a6b7c8d9e0f1a2b3c4d5e6"
        ]
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
        'method': 'debug_batchSendRawTransaction',
        'params': [
        [
            "0x02f8710182046282520894c02aaa39b223fe8d0a0e5c4f27ead9083c756cc287038d7ea4c6800080c001a01c8a4bb2b9c8b0c1d2e3f4a5b6c7d8e9f0a1b2c3d4e5f6a7b8c9d0e1f2a3b4c5a04d5e6f7a8b9c0d1e2f3a4b5c6d7e8f9a0b1c2d3e4f5a6b7c8d9e0f1a2b3c4d5e6",
            "0x02f8710182046382520894c02aaa39b223fe8d0a0e5c4f27ead9083c756cc287038d7ea4c6800080c001a02c8a4bb2b9c8b0c1d2e3f4a5b6c7d8e9f0a1b2c3d4e5f6a7b8c9d0e1f2a3b4c5a05d5e6f7a8b9c0d1e2f3a4b5c6d7e8f9a0b1c2d3e4f5a6b7c8d9e0f1a2b3c4d5e6"
        ]
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
            "method": "debug_batchSendRawTransaction",
            "params": [
        [
            "0x02f8710182046282520894c02aaa39b223fe8d0a0e5c4f27ead9083c756cc287038d7ea4c6800080c001a01c8a4bb2b9c8b0c1d2e3f4a5b6c7d8e9f0a1b2c3d4e5f6a7b8c9d0e1f2a3b4c5a04d5e6f7a8b9c0d1e2f3a4b5c6d7e8f9a0b1c2d3e4f5a6b7c8d9e0f1a2b3c4d5e6",
            "0x02f8710182046382520894c02aaa39b223fe8d0a0e5c4f27ead9083c756cc287038d7ea4c6800080c001a02c8a4bb2b9c8b0c1d2e3f4a5b6c7d8e9f0a1b2c3d4e5f6a7b8c9d0e1f2a3b4c5a05d5e6f7a8b9c0d1e2f3a4b5c6d7e8f9a0b1c2d3e4f5a6b7c8d9e0f1a2b3c4d5e6"
        ]
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
    "result": [
        "0x88df016429689c079f3b2f6ad39fa052532c56795b733da78a91ebe6a713944b",
        "0x1cd7d1e5a67c7e4f1bb2d4c50a9c27c3b4a7f9e6b7a4919c2d5b0a6f3d9e1c4b"
    ]
}
```

## Response Parameters

| Parameter | Type            | Description                                                                          |
| --------- | --------------- | ------------------------------------------------------------------------------------ |
| `jsonrpc` | string          | JSON-RPC protocol version ("2.0")                                                    |
| `id`      | string          | Request identifier matching the request                                              |
| `result`  | array of string | Array of 32-byte transaction hashes, in the same order as the submitted transactions |

## Use Cases

* **MEV Bundle Submission**: Submit multiple related transactions in one call for lower total latency
* **Batched Hot-Wallet Flows**: Automated systems drain-empty and refill wallets with sequential nonces atomically
* **High-Throughput Sequencer Feeds**: Bridge relayers submitting batches of L2 → L1 messages

## Error Handling

| Error Code | Message                            | Description                                                                      |
| ---------- | ---------------------------------- | -------------------------------------------------------------------------------- |
| -32601     | Method not found                   | `debug` module not enabled — Dedicated Node tier required                        |
| -32602     | Invalid params                     | One or more transactions have malformed hex or invalid RLP structure             |
| -32000     | Nonce too low / Insufficient funds | One or more transactions fail mempool checks — check per-tx errors in error data |

## Web3 Integration

{% tabs %}
{% tab title="Ethers.js" %}
{% code title="ethers-example.js" %}
```javascript
import { ethers } from 'ethers';

const provider = new ethers.JsonRpcProvider('https://shared.eu-central-1.getblock.io/<ACCESS-TOKEN>/');

const hashes = await provider.send('debug_batchSendRawTransaction', [[
    '<signed-tx-1>',
    '<signed-tx-2>',
]]);
console.log('Submitted:', hashes);
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

const hashes = await client.request({
    method: 'debug_batchSendRawTransaction',
    params: [['<signed-tx-1>', '<signed-tx-2>']],
});
console.log('Submitted:', hashes);
```
{% endcode %}
{% endtab %}
{% endtabs %}
