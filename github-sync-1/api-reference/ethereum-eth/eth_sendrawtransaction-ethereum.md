---
description: >-
  Example code for the eth_sendRawTransaction JSON RPC method. Сomplete guide on
  how to use eth_sendRawTransaction JSON RPC in GetBlock Web3 documentation.
---

# eth\_sendRawTransaction - Ethereum

This method submits a signed transaction to the network. The transaction must be a hex-encoded RLP-serialized signed payload — clients sign transactions locally with their private keys and send only the resulting bytes. Returns the transaction hash immediately if the transaction is well-formed and accepted into the mempool; final mining confirmation is checked separately via `eth_getTransactionReceipt`.

## Parameters

| Parameter           | Type   | Required | Description                                                      |
| ------------------- | ------ | -------- | ---------------------------------------------------------------- |
| `signedTransaction` | string | Yes      | Hex-encoded RLP-serialized signed transaction (with `0x` prefix) |

## Request

{% tabs %}
{% tab title="cURL" %}
{% code overflow="wrap" %}
```bash
curl --location --request POST 'https://shared.eu-central-1.getblock.io/<ACCESS-TOKEN>/' \
--header 'Content-Type: application/json' \
--data-raw '{
    "jsonrpc": "2.0",
    "method": "eth_sendRawTransaction",
    "params": [
        "0x02f8710182046282520894c02aaa39b223fe8d0a0e5c4f27ead9083c756cc287038d7ea4c6800080c001a01c8a4bb2b9c8b0c1d2e3f4a5b6c7d8e9f0a1b2c3d4e5f6a7b8c9d0e1f2a3b4c5a04d5e6f7a8b9c0d1e2f3a4b5c6d7e8f9a0b1c2d3e4f5a6b7c8d9e0f1a2b3c4d5e6"
    ],
    "id": "getblock.io"
}'
```
{% endcode %}
{% endtab %}

{% tab title="Axios" %}
```javascript
const axios = require('axios');

const response = await axios.post('https://shared.eu-central-1.getblock.io/<ACCESS-TOKEN>/', {
    jsonrpc: '2.0',
    method: 'eth_sendRawTransaction',
    params: [
        "0x02f8710182046282520894c02aaa39b223fe8d0a0e5c4f27ead9083c756cc287038d7ea4c6800080c001a01c8a4bb2b9c8b0c1d2e3f4a5b6c7d8e9f0a1b2c3d4e5f6a7b8c9d0e1f2a3b4c5a04d5e6f7a8b9c0d1e2f3a4b5c6d7e8f9a0b1c2d3e4f5a6b7c8d9e0f1a2b3c4d5e6"
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
        'method': 'eth_sendRawTransaction',
        'params': [
        "0x02f8710182046282520894c02aaa39b223fe8d0a0e5c4f27ead9083c756cc287038d7ea4c6800080c001a01c8a4bb2b9c8b0c1d2e3f4a5b6c7d8e9f0a1b2c3d4e5f6a7b8c9d0e1f2a3b4c5a04d5e6f7a8b9c0d1e2f3a4b5c6d7e8f9a0b1c2d3e4f5a6b7c8d9e0f1a2b3c4d5e6"
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
    
    let response = client
        .post("https://shared.eu-central-1.getblock.io/<ACCESS-TOKEN>/")
        .header("Content-Type", "application/json")
        .json(&json!({
            "jsonrpc": "2.0",
            "method": "eth_sendRawTransaction",
            "params": [
        "0x02f8710182046282520894c02aaa39b223fe8d0a0e5c4f27ead9083c756cc287038d7ea4c6800080c001a01c8a4bb2b9c8b0c1d2e3f4a5b6c7d8e9f0a1b2c3d4e5f6a7b8c9d0e1f2a3b4c5a04d5e6f7a8b9c0d1e2f3a4b5c6d7e8f9a0b1c2d3e4f5a6b7c8d9e0f1a2b3c4d5e6"
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
    "result": "0x88df016429689c079f3b2f6ad39fa052532c56795b733da78a91ebe6a713944b"
}
```

## Response Parameters

| Parameter | Type   | Description                                           |
| --------- | ------ | ----------------------------------------------------- |
| `jsonrpc` | string | JSON-RPC protocol version ("2.0")                     |
| `id`      | string | Request identifier matching the request               |
| `result`  | string | 32-byte transaction hash of the submitted transaction |

## Use Cases

* **Wallet Transaction Submission**: Submit locally-signed transactions from a wallet to the network
* **MEV Bundle Alternative**: Submit standalone transactions when not going through a private mempool relay
* **Backend Automation**: Systems using hot wallets sign and submit transactions programmatically
* **Bridge Relayer Submission**: Cross-chain bridge relayers submit signed messages as regular transactions

## Error Handling

| Error Code | Message                                    | Description                                                                           |
| ---------- | ------------------------------------------ | ------------------------------------------------------------------------------------- |
| -32602     | Invalid params                             | Signed transaction is not valid hex or the RLP structure is malformed                 |
| -32000     | Nonce too low                              | The transaction's nonce is lower than the sender's current on-chain nonce             |
| -32000     | Insufficient funds for gas × price + value | Sender doesn't have enough ETH to cover the maximum transaction cost                  |
| -32000     | Transaction underpriced                    | Transaction's gas price is below the mempool minimum or below a replacement threshold |
| -32000     | Already known                              | This transaction hash already exists in the mempool                                   |
| -32603     | Internal error                             | Node failed to process the submission                                                 |

## Web3 Integration

{% tabs %}
{% tab title="Ethers.js" %}
```javascript
import { ethers } from 'ethers';

const provider = new ethers.JsonRpcProvider('https://shared.eu-central-1.getblock.io/<ACCESS-TOKEN>/');

// Normally you'd construct and sign the transaction with a Wallet:
const wallet = new ethers.Wallet('<PRIVATE-KEY>', provider);
const tx = await wallet.sendTransaction({
    to: '0xC02aaA39b223FE8D0A0e5C4F27eAD9083C756Cc2',
    value: ethers.parseEther('0.001'),
});
console.log('Submitted:', tx.hash);

// Wait for mining:
const receipt = await tx.wait();
console.log('Mined in block:', receipt.blockNumber);
```
{% endtab %}

{% tab title="Viem" %}
```javascript
import { createPublicClient, createWalletClient, http, parseEther } from 'viem';
import { privateKeyToAccount } from 'viem/accounts';
import { mainnet } from 'viem/chains';

const account = privateKeyToAccount('<PRIVATE-KEY>');
const walletClient = createWalletClient({
    account,
    chain: mainnet,
    transport: http('https://shared.eu-central-1.getblock.io/<ACCESS-TOKEN>/'),
});

const hash = await walletClient.sendTransaction({
    to: '0xC02aaA39b223FE8D0A0E5C4F27eAD9083C756Cc2',
    value: parseEther('0.001'),
});
console.log('Submitted:', hash);
```
{% endtab %}
{% endtabs %}
