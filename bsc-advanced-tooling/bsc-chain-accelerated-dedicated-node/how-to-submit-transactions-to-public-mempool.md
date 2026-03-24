---
description: >-
  Learn how to submit transactions to the BNB Chain public mempool through
  GetBlock's BDN fast path.
---

# How to Submit Transactions to Public Mempool

This process involves submitting transactions to the BNB Chain public mempool via GetBlock's BDN fast path. Your transaction propagates to validators significantly faster than through standard P2P gossip, increasing the probability of earlier block inclusion.

### When to Use Public Mempool

Use public mempool submission when:

* You want faster propagation without hiding your transaction
* MEV exposure is acceptable for your use case
* You're competing on speed rather than privacy
* You need broad validator visibility quickly

_For MEV-sensitive transactions, see_ [_Private Transactions_](../bsc-accelerated-dedicated-node/sending-transactions-to-private-mempool-priority-fee/) _instead._

### Sample Request

{% tabs %}
{% tab title="Endpoint" %}
```bash
wss://go.getblock.io/<ACCESS_TOKEN>
```
{% endtab %}

{% tab title="Method" %}
```bash
eth_sendRawTransaction
```
{% endtab %}

{% tab title="Transaction Parameters" %}
| Parameter  | Type   | Required | Description                        |
| ---------- | ------ | -------- | ---------------------------------- |
| `nonce`    | int    | Yes      | Transaction count for your address |
| `to`       | string | Yes      | Recipient address                  |
| `value`    | BigInt | Yes      | Amount in wei                      |
| `gasPrice` | BigInt | Yes      | Gas price (minimum 3 gwei)         |
| `gasLimit` | int    | Yes      | Gas limit for execution            |
| `data`     | string | No       | Calldata for contract calls        |
| `chainId`  | int    | Yes      | `56` for BSC Mainnet               |
{% endtab %}

{% tab title="Request Format" %}
```json
{
  "jsonrpc": "2.0",
  "id": 1,
  "method": "blxr_tx",
  "params": ["0x<signed_transaction_hex>"]
}
```
{% endtab %}

{% tab title="Response Format" %}
```json
{
  "jsonrpc": "2.0",
  "id": 1,
  "result": "0x<transaction_hash>"
}
```
{% endtab %}
{% endtabs %}

### How to Submit a Transaction to Public Mempool

{% stepper %}
{% step %}
Set up the project

{% tabs %}
{% tab title="npm" %}
```bash
mkdir transaction-public-mempool
cd transaction-public-mempool
npm init -y
npm install ws ethers dotenv
```
{% endtab %}

{% tab title="yarn" %}
```bash
mkdir transaction-public-mempool
cd transaction-public-mempool
yarn init -y
yarn ws ethers dotenv
```
{% endtab %}
{% endtabs %}
{% endstep %}

{% step %}
Create a new file named `index.js`. This is where you will make your first call.
{% endstep %}

{% step %}
Set the ES module `"type": "module"` in your `package.json`.
{% endstep %}

{% step %}
Create `.env` file and add the following:

{% code overflow="wrap" %}
```js
ACCESS_TOKEN=your-accelerated-node-endpoint
RPC_URL=your-normal-bsc-node-endppoint //e.g https
PRIVATE_KEY=your-wallet-private-key
TO_ADDRESS=the-receiver-address
```
{% endcode %}
{% endstep %}

{% step %}
Add the following code to `index.js`:
{% endstep %}

{% step %}
```javascript
import WebSocket from "ws";
import { ethers } from "ethers";
import "dotenv/config";
```
{% endstep %}

{% step %}
Create and Sign Your Transaction

```javascript
const PRIVATE_KEY = process.env.PRIVATE_KEY;
const RPC_URL = process.env.RPC_URL;
const provider = new ethers.JsonRpcProvider(RPC_URL);
const wallet = new ethers.Wallet(PRIVATE_KEY, provider);

// Get current nonce
const nonce = await provider.getTransactionCount(wallet.address);
// Build transaction
const tx = {
  nonce: nonce,
  to: process.env.TO_ADDRESS,
  value: ethers.parseEther("0.001"),
  gasPrice: ethers.parseUnits("3", "gwei"), // Minimum 3 gwei for BSC
  gasLimit: 21000,
  chainId: 56, // BSC Mainnet
};
// Sign transaction
const signedTx = await wallet.signTransaction(tx); 
const signedTxNoPrefix = signedTx.startsWith("0x")
  ? signedTx.slice(2)
  : signedTx;
```
{% endstep %}

{% step %}
Submit via WebSocket

```javascript
const ws = new WebSocket(process.env.ACCESS_TOKEN);
ws.on("open", () => {
  ws.send(
    JSON.stringify({
      jsonrpc: "2.0",
      id: 1,
      method: "blxr_tx",
      params: {"transaction": signedTxNoPrefix},
    }),
  );
});
ws.on("message", (data) => {
  const response = JSON.parse(data);
  if (response.result?.txHash) {
  const hash = response.result.txHash;
  console.log("TX Hash:", hash);
  console.log(`https://bscscan.com/tx/${hash}`);
} else if (response.error) {
    console.error("❌ Error:", response.error.message);
  }
  ws.close();
});
```
{% endstep %}

{% step %}
Response

```bash
01f86c380384b2d05e0082520894d26ea0f03100358b2ebd4c9638f042aada9a1bcf87038d7ea4c6800080c001a04647f98754480337ae409bcc225f2d62b706b47f31385221421cea20e41080b0a03ae32f07bb7110c4153c2ab121c152db7d3030d54cc0c5341f0b75b20e3d439d
TX Hash: 00d4fe2db5c667ce549318cf621913af1bcb130022ec51020cbb6cae2b7eedce
https://bscscan.com/tx/00d4fe2db5c667ce549318cf621913af1bcb130022ec51020cbb6cae2b7eedce
```
{% endstep %}
{% endstepper %}

<details>

<summary>Complete Example: BNB Transfer</summary>

```javascript
import WebSocket from "ws";
import { ethers } from "ethers";
import "dotenv/config";

const PRIVATE_KEY = process.env.PRIVATE_KEY;
const RPC_URL = process.env.RPC_URL;
const provider = new ethers.JsonRpcProvider(RPC_URL);
const wallet = new ethers.Wallet(PRIVATE_KEY, provider);


// Get current nonce
const nonce = await provider.getTransactionCount(wallet.address);

// Build transaction
const tx = {
  nonce: nonce,
  to: "0xd26ea0f03100358b2Ebd4c9638f042aAda9a1BcF",
  value: ethers.parseEther("0.001"),
  gasPrice: ethers.parseUnits("3", "gwei"), // Minimum 3 gwei for BSC
  gasLimit: 21000,
  chainId: 56, // BSC Mainnet
};

// Sign transaction
const signedTx = await wallet.signTransaction(tx); // e.g. "0xabcdef..."
const signedTxNoPrefix = signedTx.startsWith("0x")
  ? signedTx.slice(2)
  : signedTx;

console.log(signedTxNoPrefix);

const ws = new WebSocket(process.env.ACCESS_TOKEN);
ws.on("open", () => {
  ws.send(
    JSON.stringify({
      jsonrpc: "2.0",
      id: 1,
      method: "blxr_tx",
      params: {"transaction": signedTxNoPrefix},
    }),
  );
});

ws.on("message", (data) => {
  const response = JSON.parse(data);
  if (response.result?.txHash) {
  const hash = response.result.txHash;
  console.log("TX Hash:", hash);
  console.log(`https://bscscan.com/tx/${hash}`);
} else if (response.error) {
    console.error("❌ Error:", response.error.message);
  }

  ws.close();
});
```

</details>

<details>

<summary>Complete Example: Token Swap on PancakeSwap</summary>

```javascript
import WebSocket from 'ws';
import { ethers } from 'ethers';
import 'dotenv/config';

const PRIVATE_KEY = process.env.PRIVATE_KEY;
const RPC_URL = process.env.RPC_URL;

const PANCAKE_ROUTER = '0x10ED43C718714eb63d5aA57B78B54704E256024E';
const WBNB = '0xbb4CdB9CBd36B01bD1cBaEBF2De08d9173bc095c';

async function swapBNBForToken(tokenAddress, amountBNB, minAmountOut) {
  const provider = new ethers.JsonRpcProvider(RPC_URL);
  const wallet = new ethers.Wallet(PRIVATE_KEY, provider);
  
  // Router ABI
  const routerABI = [
    'function swapExactETHForTokens(uint256 amountOutMin, address[] path, address to, uint256 deadline) payable'
  ];
  const router = new ethers.Contract(PANCAKE_ROUTER, routerABI, wallet);
  
  // Build swap calldata
  const deadline = Math.floor(Date.now() / 1000) + 300;  // 5 minutes
  const swapData = router.interface.encodeFunctionData('swapExactETHForTokens', [
    minAmountOut,
    [WBNB, tokenAddress],
    wallet.address,
    deadline
  ]);
  
  // Get nonce
  const nonce = await provider.getTransactionCount(wallet.address);
  
  // Build transaction
  const tx = {
    nonce: nonce,
    to: PANCAKE_ROUTER,
    value: ethers.parseEther(amountBNB),
    data: swapData,
    gasPrice: ethers.parseUnits('3', 'gwei'),
    gasLimit: 300000,  // Higher gas limit for swaps
    chainId: 56
  };
  
  // Sign transaction
  const signedTx = await wallet.signTransaction(tx);
  const signedTxNoPrefix = signedTx.startsWith("0x")
 ? signedTx.slice(2)
 : signedTx;
  
  console.log('Swap details:');
  console.log('  Amount:', amountBNB, 'BNB');
  console.log('  Token:', tokenAddress);
  console.log('  Min out:', ethers.formatEther(minAmountOut));
  
  // Submit via BDN
 const ws = new WebSocket(`wss://go.getblock.io/${process.env.ACCESS_TOKEN}`);
  
  return new Promise((resolve, reject) => {
    ws.on('open', () => {
      ws.send(JSON.stringify({
        jsonrpc: '2.0',
        id: 1,
        method: "blxr_tx",
        params: {"transaction": signedTxNoPrefix},
      }));
    });
    
    ws.on('message', (data) => {
      const response = JSON.parse(data);
      ws.close();
      
      if (response.result) {
        console.log('\n✅ Swap submitted!');
        console.log('TX Hash:', response.result);
        resolve(response.result);
      } else {
        console.error('\n❌ Error:', response.error);
        reject(response.error);
      }
    });
  });
}

// Usage: Swap 0.001 BNB for BUSD
swapBNBForToken(
  '0xe9e7CEA3DedcA5984780Bafc599bD69ADd087D56',  // BUSD
  '0.001',                                          // 0.001 BNB
  ethers.parseEther('50')                         // Min 50 BUSD
).catch(console.error);
```

</details>

### Gas Price Recommendations

| Priority | Gas Price | Use Case              |
| -------- | --------- | --------------------- |
| Standard | 3 gwei    | Regular transfers     |
| Fast     | 5 gwei    | Time-sensitive trades |
| Urgent   | 10+ gwei  | Competitive scenarios |

{% hint style="info" %}
Note: Higher gas prices increase inclusion priority but also increase transaction cost.
{% endhint %}

### Troubleshooting

1. **"nonce too low"**

Your nonce is behind the network state. Fetch the latest:

```javascript
const nonce = await provider.getTransactionCount(wallet.address, 'pending');
```

2. **"insufficient funds"**

Ensure you have enough BNB for both the transaction value and gas:

```
Required = value + (gasPrice × gasLimit)
```

3. **"replacement transaction underpriced"**

If replacing a pending transaction, increase the gas price by at least 10%:

```javascript
const newGasPrice = existingGasPrice * 110n / 100n;
```

4. **Transaction not included**

* Increase gas price for higher priority
* Check that the transaction is valid
* Verify sufficient balance

## Next Steps

For transactions that need MEV protection, see:

* [Private Transactions](../bsc-accelerated-dedicated-node/how-to-submit-transaction-to-private-mempool.md) — Hidden from public mempool
* [Private Transactions with Tips](../bsc-accelerated-dedicated-node/sending-transactions-to-private-mempool-priority-fee/) — Prioritized private submission
