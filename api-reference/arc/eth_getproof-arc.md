---
description: >-
  Example code for the eth_getProof JSON-RPC method. Complete guide on how to
  use eth_getProof JSON-RPC in GetBlock Web3 documentation.
---

# eth\_getProof - ARC

This method returns the Merkle-Patricia proof for an account and optional storage keys. On Arc, Merkle proofs combined with Malachite's deterministic finality enable strong light-client guarantees suitable for institutional bridges and compliance systems.

## Parameters

| Parameter      | Type            | Required | Description                                                                            |
| -------------- | --------------- | -------- | -------------------------------------------------------------------------------------- |
| address        | string          | Yes      | 20-byte address of the account                                                         |
| storageKeys    | array of string | Yes      | Array of 32-byte storage slot keys to include in the proof (may be empty)              |
| blockParameter | string          | Yes      | Block number in hex, or `"latest"`, `"earliest"`, `"pending"`, `"safe"`, `"finalized"` |

## Request Example

{% tabs %}
{% tab title="cURL" %}
```bash
curl --location --request POST 'https://go.getblock.io/<ACCESS-TOKEN>/' \
--header 'Content-Type: application/json' \
--data-raw '{
    "jsonrpc": "2.0",
    "method": "eth_getProof",
    "params": [
        "0x4cef52a8f9d4b0c8b8d5f1a2b3c4d5e6f7a8b9c0",
        [
            "0x0000000000000000000000000000000000000000000000000000000000000000"
        ],
        "latest"
    ],
    "id": "getblock.io"
}'
```
{% endtab %}

{% tab title="JavaScript (Axios)" %}
```javascript
import axios from 'axios';

const data = JSON.stringify({
    "jsonrpc": "2.0",
    "method": "eth_getProof",
    "params": [
        "0x4cef52a8f9d4b0c8b8d5f1a2b3c4d5e6f7a8b9c0",
        [
            "0x0000000000000000000000000000000000000000000000000000000000000000"
        ],
        "latest"
    ],
    "id": "getblock.io"
});

const config = {
    method: 'post',
    url: 'https://go.getblock.io/<ACCESS-TOKEN>/',
    headers: {
        'Content-Type': 'application/json'
    },
    data: data
};

axios(config)
    .then(response => console.log(JSON.stringify(response.data, null, 2)))
    .catch(error => console.log(error));
```
{% endtab %}

{% tab title="Python (Requests)" %}
```python
import requests
import json

url = "https://go.getblock.io/<ACCESS-TOKEN>/"

payload = json.dumps({
    "jsonrpc": "2.0",
    "method": "eth_getProof",
    "params": [
        "0x4cef52a8f9d4b0c8b8d5f1a2b3c4d5e6f7a8b9c0",
        [
            "0x0000000000000000000000000000000000000000000000000000000000000000"
        ],
        "latest"
    ],
    "id": "getblock.io"
})

headers = {
    'Content-Type': 'application/json'
}

response = requests.post(url, headers=headers, data=payload)
print(response.text)
```
{% endtab %}

{% tab title="Rust (Reqwest)" %}
```rust
use reqwest::Client;
use serde_json::json;

#[tokio::main]
async fn main() -> Result<(), Box<dyn std::error::Error>> {
    let client = Client::new();

    let payload = json!({
        "jsonrpc": "2.0",
        "method": "eth_getProof",
        "params": [
                "0x4cef52a8f9d4b0c8b8d5f1a2b3c4d5e6f7a8b9c0",
                [
                        "0x0000000000000000000000000000000000000000000000000000000000000000"
                ],
                "latest"
        ],
        "id": "getblock.io"
});

    let response = client
        .post("https://go.getblock.io/<ACCESS-TOKEN>/")
        .header("Content-Type", "application/json")
        .json(&payload)
        .send()
        .await?;

    let result: serde_json::Value = response.json().await?;
    println!("{:#?}", result);

    Ok(())
}
```
{% endtab %}
{% endtabs %}

## Response Example

{% code overflow="wrap" %}
```json
{
    "jsonrpc": "2.0",
    "id": "getblock.io",
    "result": {
        "address": "0x4cef52a8f9d4b0c8b8d5f1a2b3c4d5e6f7a8b9c0",
        "accountProof": [
            "0xf90211a0\u2026",
            "0xf8d180a0\u2026"
        ],
        "balance": "0x5f5e100",
        "codeHash": "0xc5d2460186f7233c927e7db2dcc703c0e500b653ca82273b7bfad8045d85a470",
        "nonce": "0x42",
        "storageHash": "0x56e81f171bcc55a6ff8345e692c0f86e5b48e01b996cadc001622fb5e363b421",
        "storageProof": [
            {
                "key": "0x0000000000000000000000000000000000000000000000000000000000000000",
                "value": "0x539",
                "proof": [
                    "0xf8a180\u2026"
                ]
            }
        ]
    }
}
```
{% endcode %}

## Response Parameters

| Field               | Type   | Description                                    |
| ------------------- | ------ | ---------------------------------------------- |
| result.balance      | string | Account balance in USDC base units (hex)       |
| result.nonce        | string | Account nonce (hex)                            |
| result.codeHash     | string | Keccak-256 hash of the account's code          |
| result.storageHash  | string | Root of the storage trie for this account      |
| result.accountProof | array  | Merkle proof for the account in the state trie |
| result.storageProof | array  | Merkle proofs for the requested storage keys   |

## Use Cases

* Light client verification of account state
* Cross-chain bridges that prove Arc state to other chains (CCTP integrations)
* Generating zero-knowledge proofs of state for Arc's privacy layer
* Compliance attestations on stablecoin balances

## Error Handling

| Status Code | Error Message     | Cause                                       |
| ----------- | ----------------- | ------------------------------------------- |
| 403         | Forbidden         | Missing or invalid `<ACCESS-TOKEN>`         |
| -32602      | Invalid params    | Request parameters are missing or malformed |
| -32601      | Method not found  | The method is not supported by this node    |
| 429         | Too Many Requests | Rate limit exceeded for your plan           |

## SDK Integration

{% tabs %}
{% tab title="Ethers.js" %}
```javascript
import { ethers } from 'ethers';

const provider = new ethers.JsonRpcProvider('https://go.getblock.io/<ACCESS-TOKEN>/');

// Most methods are exposed directly on the provider.
// For raw access, use provider.send(method, params).
const result = await provider.send('eth_getProof', ["0x4cef52a8f9d4b0c8b8d5f1a2b3c4d5e6f7a8b9c0", ["0x0000000000000000000000000000000000000000000000000000000000000000"], "latest"]);
console.log(result);
```
{% endtab %}

{% tab title="Viem" %}
```javascript
import { createPublicClient, http, defineChain } from 'viem';

const arcTestnet = defineChain({
    id: 5042002,
    name: 'Arc Testnet',
    network: 'arc-testnet',
    nativeCurrency: { name: 'USD Coin', symbol: 'USDC', decimals: 6 },
    rpcUrls: { default: { http: ['https://go.getblock.io/<ACCESS-TOKEN>/'] } },
    blockExplorers: { default: { name: 'arcscan', url: 'https://testnet.arcscan.app' } }
});

const client = createPublicClient({
    chain: arcTestnet,
    transport: http('https://go.getblock.io/<ACCESS-TOKEN>/')
});

const result = await client.request({ method: 'eth_getProof', params: ["0x4cef52a8f9d4b0c8b8d5f1a2b3c4d5e6f7a8b9c0", ["0x0000000000000000000000000000000000000000000000000000000000000000"], "latest"] });
console.log(result);
```
{% endtab %}
{% endtabs %}
