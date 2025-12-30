---
description: >-
  Example code for the eth_sendRawTransaction JSON-RPC method. Complete guide on
  how to use eth_sendRawTransaction JSON-RPC in GetBlock Web3 documentation.
---

# eth\_sendRawTransaction - Base

The `eth_sendRawTransaction` method submits a signed transaction to the Base network for broadcast and inclusion in a block. This is the primary method for sending transactions, including ETH transfers, contract calls, and contract deployments.

## Parameters

| Parameter             | Type   | Required | Description                                 |
| --------------------- | ------ | -------- | ------------------------------------------- |
| signedTransactionData | string | Yes      | The signed transaction data as a hex string |

## Request

{% tabs %}
{% tab title="Curl" %}
```bash
curl --location --request POST 'https://go.getblock.io/<ACCESS-TOKEN>/' \
--header 'Content-Type: application/json' \
--data-raw '{
    "jsonrpc": "2.0",
    "method": "eth_sendRawTransaction",
    "params": ["0xf86c098504a817c800825208943535353535353535353535353535353535353535880de0b6b3a76400008025a028ef61340bd939bc2195fe537567866003e1a15d3c71ff63e1590620aa636276a067cbe9d8997f761aecb703304b3800ccf555c9f3dc64214b297fb1966a3b6d83"],
    "id": "getblock.io"
}'
```
{% endtab %}

{% tab title="Axios" %}
```javascript
const axios = require('axios');

const signedTx = '0xf86c098504a817c800825208943535353535353535353535353535353535353535880de0b6b3a76400008025a028ef61340bd939bc2195fe537567866003e1a15d3c71ff63e1590620aa636276a067cbe9d8997f761aecb703304b3800ccf555c9f3dc64214b297fb1966a3b6d83';

const response = await axios.post('https://go.getblock.io/<ACCESS-TOKEN>/', {
    jsonrpc: '2.0',
    method: 'eth_sendRawTransaction',
    params: [signedTx],
    id: 'getblock.io'
}, {
    headers: { 'Content-Type': 'application/json' }
});

console.log('Transaction Hash:', response.data.result);
```
{% endtab %}

{% tab title="Request" %}
```python
import requests

signed_tx = '0xf86c098504a817c800825208943535353535353535353535353535353535353535880de0b6b3a76400008025a028ef61340bd939bc2195fe537567866003e1a15d3c71ff63e1590620aa636276a067cbe9d8997f761aecb703304b3800ccf555c9f3dc64214b297fb1966a3b6d83'

response = requests.post(
    'https://go.getblock.io/<ACCESS-TOKEN>/',
    headers={'Content-Type': 'application/json'},
    json={
        'jsonrpc': '2.0',
        'method': 'eth_sendRawTransaction',
        'params': [signed_tx],
        'id': 'getblock.io'
    }
)

result = response.json()
if 'result' in result:
    print(f'Transaction Hash: {result["result"]}')
else:
    print(f'Error: {result["error"]}')
```
{% endtab %}

{% tab title="Rust" %}
```rust
use reqwest::Client;
use serde_json::{json, Value};

#[tokio::main]
async fn main() -> Result<(), Box<dyn std::error::Error>> {
    let client = Client::new();
    let signed_tx = "0xf86c098504a817c800825208943535353535353535353535353535353535353535880de0b6b3a76400008025a028ef61340bd939bc2195fe537567866003e1a15d3c71ff63e1590620aa636276a067cbe9d8997f761aecb703304b3800ccf555c9f3dc64214b297fb1966a3b6d83";
    
    let response = client
        .post("https://go.getblock.io/<ACCESS-TOKEN>/")
        .header("Content-Type", "application/json")
        .json(&json!({
            "jsonrpc": "2.0",
            "method": "eth_sendRawTransaction",
            "params": [signed_tx],
            "id": "getblock.io"
        }))
        .send()
        .await?
        .json::<Value>()
        .await?;
    
    println!("Transaction Hash: {}", response["result"]);
    Ok(())
}
```
{% endtab %}
{% endtabs %}

### cURL

### Axios (JavaScript)

### Requests (Python)

### Rust

## Response

```json
{
    "jsonrpc": "2.0",
    "id": "getblock.io",
    "result": "0xe670ec64341771606e55d6b4ca35a1a6b75ee3d5145a99d05921026d1527331"
}
```

## Response Parameters

| Parameter | Type   | Description                                  |
| --------- | ------ | -------------------------------------------- |
| jsonrpc   | string | JSON-RPC protocol version ("2.0")            |
| id        | string | Request identifier matching the request      |
| result    | string | 32-byte transaction hash, or error if failed |

## Use Cases

* ETH Transfers: Send ETH between accounts.
* Token Transfers: Execute ERC-20 token transfers.
* Contract Interaction: Call contract functions that modify state.
* Contract Deployment: Deploy new smart contracts.
* DeFi Operations: Execute swaps, liquidity operations.

## Error Handling

| Error Code | Message                 | Description                             |
| ---------- | ----------------------- | --------------------------------------- |
| -32602     | Invalid params          | Invalid transaction data format         |
| -32603     | Internal error          | Transaction processing failed           |
| -32000     | Nonce too low           | Nonce already used                      |
| -32000     | Insufficient funds      | Not enough ETH for gas + value          |
| -32000     | Replacement underpriced | Gas price too low to replace pending tx |
| -32000     | Gas limit exceeded      | Transaction gas exceeds block limit     |

## Web3 Integration

{% tabs %}
{% tab title="Ethers.js" %}
```javascript
const { ethers } = require('ethers');

const provider = new ethers.JsonRpcProvider('https://go.getblock.io/<ACCESS-TOKEN>/');
const wallet = new ethers.Wallet('YOUR_PRIVATE_KEY', provider);

// Send ETH transfer
async function sendEth(to, amount) {
    const tx = await wallet.sendTransaction({
        to: to,
        value: ethers.parseEther(amount),
        // EIP-1559 fees (recommended for Base)
        maxFeePerGas: ethers.parseUnits('0.1', 'gwei'),
        maxPriorityFeePerGas: ethers.parseUnits('0.001', 'gwei')
    });
    
    console.log('Transaction Hash:', tx.hash);
    
    // Wait for confirmation
    const receipt = await tx.wait();
    console.log('Confirmed in block:', receipt.blockNumber);
    
    return receipt;
}

// Send contract transaction
async function sendContractTx(contractAddress, abi, functionName, args) {
    const contract = new ethers.Contract(contractAddress, abi, wallet);
    
    const tx = await contract[functionName](...args);
    console.log('Transaction Hash:', tx.hash);
    
    const receipt = await tx.wait();
    console.log('Confirmed in block:', receipt.blockNumber);
    
    return receipt;
}

// Sign and send raw transaction
async function sendRawTx(to, value, data) {
    const tx = {
        to: to,
        value: value,
        data: data,
        nonce: await provider.getTransactionCount(wallet.address),
        gasLimit: 21000n,
        maxFeePerGas: ethers.parseUnits('0.1', 'gwei'),
        maxPriorityFeePerGas: ethers.parseUnits('0.001', 'gwei'),
        chainId: 8453n // Base mainnet
    };
    
    const signedTx = await wallet.signTransaction(tx);
    const txResponse = await provider.broadcastTransaction(signedTx);
    
    return txResponse;
}
```
{% endtab %}

{% tab title="Viem" %}
```javascript
import { createWalletClient, createPublicClient, http, parseEther, parseGwei } from 'viem';
import { base } from 'viem/chains';
import { privateKeyToAccount } from 'viem/accounts';

const account = privateKeyToAccount('0xYOUR_PRIVATE_KEY');

const walletClient = createWalletClient({
    account,
    chain: base,
    transport: http('https://go.getblock.io/<ACCESS-TOKEN>/')
});

const publicClient = createPublicClient({
    chain: base,
    transport: http('https://go.getblock.io/<ACCESS-TOKEN>/')
});

// Send ETH transfer
async function sendEth(to, amount) {
    const hash = await walletClient.sendTransaction({
        to: to,
        value: parseEther(amount),
        maxFeePerGas: parseGwei('0.1'),
        maxPriorityFeePerGas: parseGwei('0.001')
    });
    
    console.log('Transaction Hash:', hash);
    
    // Wait for confirmation
    const receipt = await publicClient.waitForTransactionReceipt({ hash });
    console.log('Confirmed in block:', receipt.blockNumber);
    
    return receipt;
}

// Send contract transaction
async function sendContractTx(contractAddress, abi, functionName, args) {
    const { request } = await publicClient.simulateContract({
        address: contractAddress,
        abi: abi,
        functionName: functionName,
        args: args,
        account
    });
    
    const hash = await walletClient.writeContract(request);
    console.log('Transaction Hash:', hash);
    
    const receipt = await publicClient.waitForTransactionReceipt({ hash });
    return receipt;
}

// Sign and send raw transaction
async function sendRawTx(to, value, data) {
    const signedTx = await walletClient.signTransaction({
        to: to,
        value: value,
        data: data,
        nonce: await publicClient.getTransactionCount({ address: account.address }),
        gas: 21000n,
        maxFeePerGas: parseGwei('0.1'),
        maxPriorityFeePerGas: parseGwei('0.001')
    });
    
    const hash = await walletClient.sendRawTransaction({
        serializedTransaction: signedTx
    });
    
    return hash;
}
```
{% endtab %}
{% endtabs %}
