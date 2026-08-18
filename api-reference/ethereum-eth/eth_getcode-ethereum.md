---
description: >-
  Example code for the eth_getCode JSON RPC method. Сomplete guide on how to use
  eth_getCode JSON RPC in GetBlock Web3 documentation.
---

# eth\_getCode - Ethereum

This method returns the deployed bytecode at a specific address, hex-encoded. For contract addresses it returns the runtime bytecode; for EOAs it returns `0x`. Post-Pectra (EIP-7702) EOAs can temporarily delegate to smart-contract code, in which case this returns the delegated code marker (`0xef0100` + implementation address).

## Parameters

| Parameter        | Type   | Required | Description                                                                  |
| ---------------- | ------ | -------- | ---------------------------------------------------------------------------- |
| `address`        | string | Yes      | 20-byte address to query (hex-encoded with `0x` prefix)                      |
| `blockParameter` | string | Yes      | Block number in hex, or `latest`, `earliest`, `pending`, `finalized`, `safe` |

## Request

{% tabs %}
{% tab title="cURL" %}
```bash
curl --location --request POST 'https://shared.eu-central-1.getblock.io/<ACCESS-TOKEN>/' \
--header 'Content-Type: application/json' \
--data-raw '{
    "jsonrpc": "2.0",
    "method": "eth_getCode",
    "params": [
        "0xC02aaA39b223FE8D0A0e5C4F27eAD9083C756Cc2",
        "latest"
    ],
    "id": "getblock.io"
}'
```
{% endtab %}

{% tab title="Axios" %}
```javascript
const axios = require('axios');

const response = await axios.post('https://shared.eu-central-1.getblock.io/<ACCESS-TOKEN>/', {
    jsonrpc: '2.0',
    method: 'eth_getCode',
    params: [
        "0xC02aaA39b223FE8D0A0e5C4F27eAD9083C756Cc2",
        "latest"
    ],
    id: 'getblock.io'
}, {
    headers: { 'Content-Type': 'application/json' }
});

console.log(response.data.result);
```
{% endtab %}

{% tab title="Request" %}
```python
import requests

response = requests.post(
    'https://shared.eu-central-1.getblock.io/<ACCESS-TOKEN>/',
    headers={'Content-Type': 'application/json'},
    json={
        'jsonrpc': '2.0',
        'method': 'eth_getCode',
        'params': [
        "0xC02aaA39b223FE8D0A0e5C4F27eAD9083C756Cc2",
        "latest"
    ],
        'id': 'getblock.io'
    }
)

print(response.json())
```
{% endtab %}

{% tab title="Rust" %}
```rust
use reqwest::Client;
use serde_json::{json, Value};

#[tokio::main]
async fn main() -> Result<(), Box<dyn std::error::Error>> {
    let client = Client::new();
    
    const response = client
        .post("https://shared.eu-central-1.getblock.io/<ACCESS-TOKEN>/")
        .header("Content-Type", "application/json")
        .json(&json!({
            "jsonrpc": "2.0",
            "method": "eth_getCode",
            "params": [
        "0xC02aaA39b223FE8D0A0e5C4F27eAD9083C756Cc2",
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
{% endtab %}
{% endtabs %}

## Response

```json
{
    "jsonrpc": "2.0",
    "id": "getblock.io",
    "result": "0x6060604052361561015a5763ffffffff7c010000000000000000000000000000000000000000000000000000000060003504166306fdde0381146101a0578063095ea7b31461022a57806318160ddd1461024b57806323b872dd1461025457806326782247146102db578063313ce56714610304578063352389f31461030d5780633950935114610319578063452a9320146103375780635c60da1b1461034057806361d027b31461034957806366188463146103525780636352211e1461037057806366188463146103b25780636352211e146103d05780636db27d5c146103e457806370a0823114610411575050"
}
```

## Response Parameters

| Parameter | Type   | Description                                                                                                    |
| --------- | ------ | -------------------------------------------------------------------------------------------------------------- |
| `jsonrpc` | string | JSON-RPC protocol version ("2.0")                                                                              |
| `id`      | string | Request identifier matching the request                                                                        |
| `result`  | string | Hex-encoded contract runtime bytecode. `0x` for EOAs. Post-Pectra: `0xef0100...` for delegated EOAs (EIP-7702) |

## Use Cases

* **Contract Existence Check**: Detect whether an address is a contract (non-empty code) or an EOA (`0x`)
* **EIP-7702 Delegation Detection**: Detect when an EOA has delegated to smart-contract code by checking for the `0xef0100` prefix (post-Pectra)
* **Contract Verification**: Compare deployed bytecode against expected/compiled bytecode to detect tampering
* **Bytecode Analysis**: Feed contract bytecode into tools like `evm-disassembler`, `heimdall`, or `Panoramix` for reverse engineering

## Error Handling

| Error Code | Message        | Description                                                 |
| ---------- | -------------- | ----------------------------------------------------------- |
| -32602     | Invalid params | Address is malformed, or block parameter is invalid         |
| -32603     | Internal error | Node failed to look up account state at the requested block |

## Web3 Integration

{% tabs %}
{% tab title="Ethers.js" %}
```javascript
import { ethers } from 'ethers';

const provider = new ethers.JsonRpcProvider('https://shared.eu-central-1.getblock.io/<ACCESS-TOKEN>/');

const code = await provider.getCode('0xC02aaA39b223FE8D0A0e5C4F27eAD9083C756Cc2');

if (code === '0x') {
    console.log('EOA (no contract code)');
} else if (code.startsWith('0xef0100')) {
    // EIP-7702: EOA has delegated to a smart contract
    const delegateAddr = '0x' + code.slice(8);
    console.log('EIP-7702 delegated EOA — delegate:', delegateAddr);
} else {
    console.log('Contract, bytecode length:', (code.length - 2) / 2, 'bytes');
}
```
{% endtab %}

{% tab title="Viem" %}
```javascript
import { createPublicClient, http } from 'viem';
import { mainnet } from 'viem/chains';

const client = createPublicClient({
    chain: mainnet,
    transport: http('https://shared.eu-central-1.getblock.io/<ACCESS-TOKEN>/'),
});

const code = await client.getCode({ address: '0xC02aaA39b223FE8D0A0e5C4F27eAD9083C756Cc2' });

if (!code || code === '0x') {
    console.log('EOA (no contract code)');
} else if (code.startsWith('0xef0100')) {
    const delegateAddr = '0x' + code.slice(8);
    console.log('EIP-7702 delegated EOA — delegate:', delegateAddr);
} else {
    console.log('Contract, bytecode length:', (code.length - 2) / 2, 'bytes');
}
```
{% endtab %}
{% endtabs %}
