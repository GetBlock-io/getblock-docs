---
description: >-
  Example code for the eth_getProof JSON RPC method. Сomplete guide on how to
  use eth_getProof JSON RPC in GetBlock Web3 documentation.
---

# eth\_getProof - Ethereum

This method returns a Merkle-Patricia proof for an account and (optionally) a set of storage slots, per EIP-1186. The proof allows verifiers to cryptographically validate account state (nonce, balance, code hash, storage root) and specific storage values against a known state root — the foundation for light clients, cross-chain state proofs, and trust-minimized bridges.

## Parameters

| Parameter        | Type            | Required | Description                                                                              |
| ---------------- | --------------- | -------- | ---------------------------------------------------------------------------------------- |
| `address`        | string          | Yes      | 20-byte address to prove (hex-encoded with `0x` prefix)                                  |
| `storageKeys`    | array of string | Yes      | Array of 32-byte storage keys to include proofs for (empty array for account-only proof) |
| `blockParameter` | string          | Yes      | Block number in hex, or `latest`, `earliest`, `pending`, `finalized`, `safe`             |

## Request

{% tabs %}
{% tab title="cURL" %}
{% code overflow="wrap" %}
```bash
curl --location --request POST 'https://shared.eu-central-1.getblock.io/<ACCESS-TOKEN>/' \
--header 'Content-Type: application/json' \
--data-raw '{
    "jsonrpc": "2.0",
    "method": "eth_getProof",
    "params": [
        "0xC02aaA39b223FE8D0A0e5C4F27eAD9083C756Cc2",
        [
            "0x0000000000000000000000000000000000000000000000000000000000000000"
        ],
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
    method: 'eth_getProof',
    params: [
        "0xC02aaA39b223FE8D0A0e5C4F27eAD9083C756Cc2",
        [
            "0x0000000000000000000000000000000000000000000000000000000000000000"
        ],
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
        'method': 'eth_getProof',
        'params': [
            "0xC02aaA39b223FE8D0A0e5C4F27eAD9083C756Cc2",
            [
                "0x0000000000000000000000000000000000000000000000000000000000000000"
            ],
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
            "method": "eth_getProof",
            "params": [
                "0xC02aaA39b223FE8D0A0e5C4F27eAD9083C756Cc2",
                [
                    "0x0000000000000000000000000000000000000000000000000000000000000000"
                ],
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
    "result": {
        "address": "0xC02aaA39b223FE8D0A0e5C4F27eAD9083C756Cc2",
        "accountProof": [
            "0xf90211a01dcc4de8dec75d7aab85b567b6ccd41ad312451b948a7413f0a142fd40d49347...",
            "0xf90211a08a2b1c9d8e7f6a5b4c3d2e1f0a9b8c7d6e5f4a3b2c1d0e9f8a7b6c5d4e3f2a1b..."
        ],
        "balance": "0x0",
        "codeHash": "0xa5f2a63f7c2c07681d05f4a3ce4bc9fdd7f6c1a2b3e4d5c6f7a8b9c0d1e2f3a4",
        "nonce": "0x1",
        "storageHash": "0xd7f3b2c8e6a1f4b5c9d0e7f8a2b3c4d5e6f7a8b9c0d1e2f3a4b5c6d7e8f9a0b1",
        "storageProof": [
            {
                "key": "0x0000000000000000000000000000000000000000000000000000000000000000",
                "value": "0xa",
                "proof": [
                    "0xf90111a09b8c7d6e5f4a3b2c1d0e9f8a7b6c5d4e3f2a1b09b8c7d6e5f4a3b2c1d0e9f8a7..."
                ]
            }
        ]
    }
}
```

## Response Parameters

| Parameter                     | Type            | Description                                                                                                         |
| ----------------------------- | --------------- | ------------------------------------------------------------------------------------------------------------------- |
| `jsonrpc`                     | string          | JSON-RPC protocol version ("2.0")                                                                                   |
| `id`                          | string          | Request identifier matching the request                                                                             |
| `result.address`              | string          | The proven address                                                                                                  |
| `result.accountProof`         | array of string | Merkle-Patricia proof nodes for the account, from root to leaf                                                      |
| `result.balance`              | string          | Account balance (wei, hex)                                                                                          |
| `result.codeHash`             | string          | keccak256 of the deployed bytecode (or 0xc5d2460186f7233c927e7db2dcc703c0e500b653ca82273b7bfad8045d85a470 for EOAs) |
| `result.nonce`                | string          | Account nonce (hex)                                                                                                 |
| `result.storageHash`          | string          | Root of the account's storage trie                                                                                  |
| `result.storageProof`         | array of object | Per-slot storage proofs (empty if no storage keys requested)                                                        |
| `result.storageProof[].key`   | string          | The storage key                                                                                                     |
| `result.storageProof[].value` | string          | The value at that slot                                                                                              |
| `result.storageProof[].proof` | array of string | Merkle-Patricia proof nodes for the slot                                                                            |

## Use Cases

* **Trust-Minimized Bridges**: Cross-chain messaging protocols verify Ethereum state on other chains via Merkle proofs
* **Light Client Validation**: Light clients confirm specific account or storage values without downloading full state
* **Historical State Attestations**: Zero-knowledge proof systems attest to Ethereum state as inputs for off-chain computation
* **Storage-Value Proofs**: Prove a specific ERC-20 balance or governance vote at a historical block for airdrops or snapshots

## Error Handling

| Error Code | Message        | Description                                                                                |
| ---------- | -------------- | ------------------------------------------------------------------------------------------ |
| -32602     | Invalid params | Address or storage keys malformed                                                          |
| -32603     | Internal error | Node failed to generate the proof (usually indicates state pruning of the requested block) |

## Web3 Integration

{% tabs %}
{% tab title="Ethers.js" %}
{% code title="ethers-example.js" %}
```javascript
import { ethers } from 'ethers';

const provider = new ethers.JsonRpcProvider('https://shared.eu-central-1.getblock.io/<ACCESS-TOKEN>/');

const proof = await provider.send('eth_getProof', [
    '0xC02aaA39b223FE8D0A0e5C4F27eAD9083C756Cc2',
    ['0x0000000000000000000000000000000000000000000000000000000000000000'],
    'latest'
]);
console.log('Balance:', proof.balance);
console.log('Storage hash:', proof.storageHash);
console.log('Account proof length:', proof.accountProof.length);
console.log('Storage proof for slot 0:', proof.storageProof[0]);
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

const proof = await client.getProof({
    address: '0xC02aaA39b223FE8D0A0e5C4F27eAD9083C756Cc2',
    storageKeys: ['0x0000000000000000000000000000000000000000000000000000000000000000'],
});
console.log('Balance:', proof.balance);
console.log('Storage hash:', proof.storageHash);
console.log('Account proof length:', proof.accountProof.length);
```
{% endcode %}
{% endtab %}
{% endtabs %}
