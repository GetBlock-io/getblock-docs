---
description: >-
  Learn how to submit a private transactions directly to block builders,
  protecting you from MEV extraction until it's included in a block.
---

# How to Submit Private Transactions Without Tips

When you submit a transaction to the public mempool, it's visible to everyone. MEV bots can:

* **Sandwich your trade** — Place orders before and after yours to extract value
* **Front-run you** — Copy your trade and execute it first
* **Back-run you** — Profit from the price impact you create

Meanwhile, Private transactions eliminate this exposure by hiding your transactions until they are included.

<figure><img src="../../.gitbook/assets/image (9).png" alt="" width="563"><figcaption></figcaption></figure>

## Private TX vs Public Mempool

| Aspect          | Public Mempool              | Private TX             |
| --------------- | --------------------------- | ---------------------- |
| Visibility      | Everyone sees it            | Only builders see it   |
| MEV exposure    | Vulnerable                  | Protected              |
| Inclusion speed | Gas price dependent         | Builder dependent      |
| Best for        | Speed-competitive scenarios | Value-sensitive trades |

## When to Use Private Transactions

Use private transactions when:

* Executing large swaps that could be sandwiched
* Trading tokens with low liquidity
* Running strategies you don't want copied
* Protecting any transaction from front-running

## API Reference

{% tabs %}
{% tab title="Endpoint" %}
```bash
wss://bsc.getblock.io/mev/ws?api_key=YOUR_API_KEY
```
{% endtab %}

{% tab title="Method" %}
```
bsc_privateTx
```
{% endtab %}

{% tab title="Parameters" %}
| Parameter      | Type   | Required | Description                             |
| -------------- | ------ | -------- | --------------------------------------- |
| `transaction`  | string | Yes      | Signed raw transaction hex              |
| `mev_builders` | array  | No       | Builders to send to. Default: `["all"]` |
{% endtab %}

{% tab title="Request Format" %}
```json
{
  "jsonrpc": "2.0",
  "id": 1,
  "method": "bsc_privateTx",
  "params": {
    "transaction": "0xf86c...",
    "mev_builders": ["all"]
  }
}
```


{% endtab %}

{% tab title="Response Format" %}
```json
{
  "jsonrpc": "2.0",
  "id": 1,
  "result": {
    "txHash": "0x..."
  }
}
```
{% endtab %}
{% endtabs %}

### Quickstart

{% stepper %}
{% step %}
Set up the project

{% tabs %}
{% tab title="npm" %}
```bash
mkdir multicall3-example
cd multicall3-example
npm init -y
npm install ws ethers dotenv
```
{% endtab %}

{% tab title="yarn" %}
```bash
mkdir multicall3-example
cd multicall3-example
yarn init -y
yarn ws ethers
```
{% endtab %}
{% endtabs %}
{% endstep %}

{% step %}
Set the ES module `"type": "module"` in your package.json.
{% endstep %}

{% step %}
Create a file and name it `index.js`
{% endstep %}

{% step %}
Add the following code to `index.js:`
{% endstep %}

{% step %}
Import the dependencies

```javascript
const WebSocket = require('ws');
const { ethers } = require('ethers');

const API_KEY = 'YOUR_API_KEY';
const PRIVATE_KEY = 'YOUR_PRIVATE_KEY';
const RPC_URL = 'https://bsc-dataseed.binance.org';
```
{% endstep %}

{% step %}
Create and Sign Your Transaction

```javascript
const provider = new ethers.JsonRpcProvider(RPC_URL);
const wallet = new ethers.Wallet(PRIVATE_KEY, provider);

// Get current nonce
const nonce = await provider.getTransactionCount(wallet.address);

// Build transaction
const tx = {
  nonce: nonce,
  to: '0xRECIPIENT_ADDRESS',
  value: ethers.parseEther('0.1'),
  gasPrice: ethers.parseUnits('3', 'gwei'),
  gasLimit: 21000,
  chainId: 56
};

// Sign transaction
const signedTx = await wallet.signTransaction(tx);
```
{% endstep %}

{% step %}
Submit Privately

```javascript
const ws = new WebSocket(`wss://bsc.getblock.io/mev/ws?api_key=${API_KEY}`);

ws.on('open', () => {
  ws.send(JSON.stringify({
    jsonrpc: '2.0',
    id: 1,
    method: 'bsc_privateTx',
    params: {
      transaction: signedTx,
      mev_builders: ['all']
    }
  }));
});

ws.on('message', (data) => {
  const response = JSON.parse(data);
  
  if (response.result) {
    console.log('✅ Private TX submitted!');
    console.log('TX Hash:', response.result.txHash);
  } else {
    console.error('❌ Error:', response.error);
  }
  
  ws.close();
});
```
{% endstep %}

{% step %}
Response

```
```
{% endstep %}
{% endstepper %}

<details>

<summary>Complete Example: Private BNB Transfer</summary>

```javascript
const WebSocket = require('ws');
const { ethers } = require('ethers');

const API_KEY = 'YOUR_API_KEY';
const PRIVATE_KEY = 'YOUR_PRIVATE_KEY';
const RPC_URL = 'https://bsc-dataseed.binance.org';

async function sendPrivateTransaction(recipient, amountBNB) {
  const provider = new ethers.JsonRpcProvider(RPC_URL);
  const wallet = new ethers.Wallet(PRIVATE_KEY, provider);
  
  console.log('Wallet:', wallet.address);
  console.log('Recipient:', recipient);
  console.log('Amount:', amountBNB, 'BNB');
  console.log('Mode: Private (MEV Protected)');
  
  // Get nonce
  const nonce = await provider.getTransactionCount(wallet.address);
  
  // Build transaction
  const signedTx = await wallet.signTransaction({
    nonce: nonce,
    to: recipient,
    value: ethers.parseEther(amountBNB),
    gasPrice: ethers.parseUnits('3', 'gwei'),
    gasLimit: 21000,
    chainId: 56
  });
  
  // Submit privately
  const ws = new WebSocket(`wss://bsc.getblock.io/mev/ws?api_key=${API_KEY}`);
  
  return new Promise((resolve, reject) => {
    ws.on('open', () => {
      console.log('\nSubmitting private transaction...');
      
      ws.send(JSON.stringify({
        jsonrpc: '2.0',
        id: 1,
        method: 'bsc_privateTx',
        params: {
          transaction: signedTx,
          mev_builders: ['all']
        }
      }));
    });
    
    ws.on('message', (data) => {
      const response = JSON.parse(data);
      ws.close();
      
      if (response.result) {
        console.log('\n✅ Private transaction submitted!');
        console.log('TX Hash:', response.result.txHash);
        console.log('BSCScan:', `https://bscscan.com/tx/${response.result.txHash}`);
        resolve(response.result.txHash);
      } else {
        console.error('\n❌ Error:', response.error);
        reject(response.error);
      }
    });
    
    ws.on('error', reject);
  });
}

// Usage
sendPrivateTransaction(
  '0xRECIPIENT_ADDRESS',
  '0.1'
).catch(console.error);
```

</details>

<details>

<summary>Complete Example: MEV-Protected PancakeSwap Trade</summary>

```javascript
const WebSocket = require('ws');
const { ethers } = require('ethers');

const API_KEY = 'YOUR_API_KEY';
const PRIVATE_KEY = 'YOUR_PRIVATE_KEY';
const RPC_URL = 'https://bsc-dataseed.binance.org';

const PANCAKE_ROUTER = '0x10ED43C718714eb63d5aA57B78B54704E256024E';
const WBNB = '0xbb4CdB9CBd36B01bD1cBaEBF2De08d9173bc095c';

async function privateSwap(tokenAddress, amountBNB, minAmountOut) {
  const provider = new ethers.JsonRpcProvider(RPC_URL);
  const wallet = new ethers.Wallet(PRIVATE_KEY, provider);
  
  // Router ABI
  const routerABI = [
    'function swapExactETHForTokens(uint256 amountOutMin, address[] path, address to, uint256 deadline) payable'
  ];
  const router = new ethers.Contract(PANCAKE_ROUTER, routerABI, wallet);
  
  // Build swap calldata
  const deadline = Math.floor(Date.now() / 1000) + 300;
  const swapData = router.interface.encodeFunctionData('swapExactETHForTokens', [
    minAmountOut,
    [WBNB, tokenAddress],
    wallet.address,
    deadline
  ]);
  
  // Get nonce
  const nonce = await provider.getTransactionCount(wallet.address);
  
  // Sign transaction
  const signedTx = await wallet.signTransaction({
    nonce: nonce,
    to: PANCAKE_ROUTER,
    value: ethers.parseEther(amountBNB),
    data: swapData,
    gasPrice: ethers.parseUnits('3', 'gwei'),
    gasLimit: 300000,
    chainId: 56
  });
  
  console.log('Swap details:');
  console.log('  Amount:', amountBNB, 'BNB');
  console.log('  Token:', tokenAddress);
  console.log('  Min out:', ethers.formatEther(minAmountOut));
  console.log('  Mode: Private (MEV Protected)');
  
  // Submit privately
  const ws = new WebSocket(`wss://bsc.getblock.io/mev/ws?api_key=${API_KEY}`);
  
  return new Promise((resolve, reject) => {
    ws.on('open', () => {
      ws.send(JSON.stringify({
        jsonrpc: '2.0',
        id: 1,
        method: 'bsc_privateTx',
        params: {
          transaction: signedTx,
          mev_builders: ['all']
        }
      }));
    });
    
    ws.on('message', (data) => {
      const response = JSON.parse(data);
      ws.close();
      
      if (response.result) {
        console.log('\n✅ Private swap submitted!');
        console.log('TX Hash:', response.result.txHash);
        console.log('BSCScan:', `https://bscscan.com/tx/${response.result.txHash}`);
        resolve(response.result.txHash);
      } else {
        console.error('\n❌ Error:', response.error);
        reject(response.error);
      }
    });
  });
}

// Usage: Swap 1 BNB for BUSD (MEV protected)
privateSwap(
  '0xe9e7CEA3DedcA5984780Bafc599bD69ADd087D56',  // BUSD
  '1.0',                                          // 1 BNB
  ethers.parseEther('200')                        // Min 200 BUSD
).catch(console.error);
```

</details>

### Selecting Specific Builders

By default, `mev_builders: ['all']` sends to all available builders. You can target specific builders instead:

```javascript
params: {
  transaction: signedTx,
  mev_builders: ['48club', 'bloxroute']  // Specific builders only
}
```

Available builders vary by network conditions. Use `['all']` for maximum inclusion probability.

### Troubleshooting

| Problem                  | Solution                                                                                                                                                                                                                                                                                                                                                                                                            |
| ------------------------ | ------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| Transaction not included | <p></p><p>Without a priority tip, private transactions compete based on gas fees alone. If your transaction isn't being included:</p><ul><li>Increase gas price — Higher gas = higher priority for builders</li><li>Add a priority tip — See <a href="sending-private-transactions-priority-fee/">Private Transactions with Tips</a></li><li>Try specific builders — Some builders may be more responsive</li></ul> |
| "Invalid transaction"    | <p></p><ul><li>Verify your transaction is properly signed</li><li>Check the chain ID is <code>56</code> (BSC Mainnet)</li><li>Ensure the nonce is correct</li></ul>                                                                                                                                                                                                                                                 |
| Connection issues        | <p></p><ul><li>Verify your API key is valid</li><li>Check network connectivity</li><li>Try reconnecting after a few seconds</li></ul>                                                                                                                                                                                                                                                                               |

### Next Steps

To increase inclusion priority for your private transactions, see [Private Transactions with Tips.](sending-private-transactions-priority-fee/)
