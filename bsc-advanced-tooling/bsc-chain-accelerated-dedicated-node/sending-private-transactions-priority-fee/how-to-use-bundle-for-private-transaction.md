---
description: >-
  This guide explains how to submit transaction bundles on BNB Smart Chain using
  GetBlock's MEV endpoint.
---

# How to use Bundle for Private Transaction

A bundle is a group of transactions submitted together with a guarantee that either all transactions execute in sequence or none are included. This atomic execution model supports several advanced use cases:

* **Arbitrage** — Execute a buy on one DEX and a sell on another within the same block, ensuring both trades complete or neither does
* **Liquidations** — Check a position's health and liquidate it atomically, preventing front-running
* **Complex strategies** — Coordinate multi-step transactions with guaranteed execution order

### Prerequisites

Before submitting bundles, ensure you have:

* A GetBlock API key with MEV endpoint access
* Node.js v16 or later
* A funded BSC wallet with sufficient BNB for all transactions and gas

### API Reference

{% tabs %}
{% tab title="Endpoint" %}
Connect to the MEV WebSocket endpoint with your API key:

```bash
wss://bsc.getblock.io/mev/ws?api_key=YOUR_API_KEY
```
{% endtab %}

{% tab title="Parameters" %}
| Parameter      | Type    | Required | Description                                                                         |
| -------------- | ------- | -------- | ----------------------------------------------------------------------------------- |
| `transactions` | array   | Yes      | Array of signed raw transactions (hex-encoded)                                      |
| `mev_builders` | object  | No       | Target builders. Use `{"all": ""}` for all builders, or specify individual builders |
| `blocks_count` | integer | No       | Number of blocks the bundle remains valid. Default: 5, Maximum: 20                  |
{% endtab %}

{% tab title="Request Format" %}
Submit bundles using the `mev_sendBundle` method:

```json
{
  "jsonrpc": "2.0",
  "id": 1,
  "method": "mev_sendBundle",
  "params": {
    "transactions": ["0xf86c...", "0xf86c..."],
    "mev_builders": { "all": "" },
    "blocks_count": 5
  }
}
```
{% endtab %}

{% tab title="Response Format" %}
A successful submission returns a bundle hash:

```json
{
  "jsonrpc": "2.0",
  "id": 1,
  "result": {
    "bundleHash": "0x..."
  }
}
```
{% endtab %}
{% endtabs %}

### Prerequisites

You must have the following:

* Node.js v18 or later installed
* A funded BSC wallet with sufficient BNB for transactions and gas
* GetBlock's BSC Accelerated Dedication Node

### Quickstart

{% stepper %}
{% step %}
Set up the project

{% tabs %}
{% tab title="npm" %}
```bash
mkdir bundle-example
cd bundle-example
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
Create a new file named `index.js`. This is where you will make your first call.
{% endstep %}

{% step %}
Set the ES module `"type": "module"` in your `package.json`.
{% endstep %}

{% step %}
Add the following code to `index.js`:

The following example demonstrates a simple BNB transfer with a priority fee:

{% code overflow="wrap" %}
```js
const WebSocket = require('ws');
const { ethers } = require('ethers');

const API_KEY = 'YOUR_API_KEY';
const PRIVATE_KEY = 'YOUR_PRIVATE_KEY';

async function submitBundle() {
  const provider = new ethers.JsonRpcProvider('https://bsc-dataseed.binance.org');
  const wallet = new ethers.Wallet(PRIVATE_KEY, provider);
  
  // Get the current nonce for your wallet
  const nonce = await provider.getTransactionCount(wallet.address);
  
  // Transaction 1: First operation
  const tx1 = await wallet.signTransaction({
    nonce: nonce,
    to: DEX_A,
    value: ethers.parseEther('1.0'),
    data: buyCalldata,
    gasPrice: ethers.parseUnits('3', 'gwei'),
    gasLimit: 300000,
    chainId: 56
  });
  
  // Transaction 2: Second operation (must use nonce + 1)
  const tx2 = await wallet.signTransaction({
    nonce: nonce + 1,
    to: DEX_B,
    value: 0,
    data: sellCalldata,
    gasPrice: ethers.parseUnits('3', 'gwei'),
    gasLimit: 300000,
    chainId: 56
  });
  
  // Connect to the MEV endpoint
  const ws = new WebSocket(`wss://bsc.getblock.io/mev/ws?api_key=${API_KEY}`);
  
  ws.on('open', () => {
    ws.send(JSON.stringify({
      jsonrpc: '2.0',
      id: 1,
      method: 'mev_sendBundle',
      params: {
        transactions: [tx1, tx2],
        mev_builders: { all: '' },
        blocks_count: 5
      }
    }));
  });
  
  ws.on('message', (data) => {
    const response = JSON.parse(data);
    
    if (response.result) {
      console.log('Bundle submitted successfully');
      console.log('Bundle Hash:', response.result.bundleHash);
    } else {
      console.error('Submission failed:', response.error);
    }
    
    ws.close();
  });
}

submitBundle();
```
{% endcode %}
{% endstep %}

{% step %}
Run the code using this command:

```bash
node index.js
```
{% endstep %}

{% step %}
Sample response

```
```
{% endstep %}
{% endstepper %}

### Example: DEX Arbitrage

<details>

<summary>This example demonstrates a complete arbitrage strategy: buying a token on PancakeSwap V2 and selling on PancakeSwap V3 within the same block, with an optional priority fee.</summary>

#### Reference Addresses

| Address                                      | Purpose                |
| -------------------------------------------- | ---------------------- |
| `0x10ED43C718714eb63d5aA57B78B54704E256024E` | PancakeSwap V2 Router  |
| `0x13f4EA83D0bd40E75C8222255bc855a974568Dd4` | PancakeSwap V3 Router  |
| `0xbb4CdB9CBd36B01bD1cBaEBF2De08d9173bc095c` | WBNB                   |
| `0x6374Ca2da5646C73Eb444aB99780495d61035f9b` | Priority fee recipient |

#### Code Example

{% code overflow="wrap" %}
```javascript
const WebSocket = require('ws');
const { ethers } = require('ethers');

const API_KEY = 'YOUR_API_KEY';
const PRIVATE_KEY = 'YOUR_PRIVATE_KEY';

// Contract addresses
const PANCAKE_V2_ROUTER = '0x10ED43C718714eb63d5aA57B78B54704E256024E';
const PANCAKE_V3_ROUTER = '0x13f4EA83D0bd40E75C8222255bc855a974568Dd4';
const FEE_ADDRESS = '0x6374Ca2da5646C73Eb444aB99780495d61035f9b';
const WBNB = '0xbb4CdB9CBd36B01bD1cBaEBF2De08d9173bc095c';
const TOKEN = '0xYOUR_TOKEN';

async function arbitrage() {
  const provider = new ethers.JsonRpcProvider('https://bsc-dataseed.binance.org');
  const wallet = new ethers.Wallet(PRIVATE_KEY, provider);
  
  const nonce = await provider.getTransactionCount(wallet.address);
  const deadline = Math.floor(Date.now() / 1000) + 300;
  
  // Encode the V2 swap
  const v2ABI = [
    'function swapExactETHForTokens(uint256 amountOutMin, address[] path, address to, uint256 deadline) payable'
  ];
  const v2Router = new ethers.Contract(PANCAKE_V2_ROUTER, v2ABI, wallet);
  
  const buyData = v2Router.interface.encodeFunctionData('swapExactETHForTokens', [
    0,  // amountOutMin (set appropriately in production)
    [WBNB, TOKEN],
    wallet.address,
    deadline
  ]);
  
  // Transaction 1: Buy on PancakeSwap V2
  const tx1 = await wallet.signTransaction({
    nonce: nonce,
    to: PANCAKE_V2_ROUTER,
    value: ethers.parseEther('1.0'),
    data: buyData,
    gasPrice: ethers.parseUnits('3', 'gwei'),
    gasLimit: 300000,
    chainId: 56
  });
  
  // Transaction 2: Sell on PancakeSwap V3
  // Note: V3 uses a different interface; replace with your actual V3 swap calldata
  const tx2 = await wallet.signTransaction({
    nonce: nonce + 1,
    to: PANCAKE_V3_ROUTER,
    value: 0,
    data: '0x...',  // Your V3 swap calldata
    gasPrice: ethers.parseUnits('3', 'gwei'),
    gasLimit: 300000,
    chainId: 56
  });
  
  // Transaction 3: Priority fee (optional but recommended)
  const tx3 = await wallet.signTransaction({
    nonce: nonce + 2,
    to: FEE_ADDRESS,
    value: ethers.parseEther('0.001'),
    gasPrice: ethers.parseUnits('3', 'gwei'),
    gasLimit: 21000,
    chainId: 56
  });
  
  // Submit the bundle
  const ws = new WebSocket(`wss://bsc.getblock.io/mev/ws?api_key=${API_KEY}`);
  
  return new Promise((resolve, reject) => {
    ws.on('open', () => {
      ws.send(JSON.stringify({
        jsonrpc: '2.0',
        id: 1,
        method: 'mev_sendBundle',
        params: {
          transactions: [tx1, tx2, tx3],
          mev_builders: { all: '' },
          blocks_count: 3  // Short validity for time-sensitive arbitrage
        }
      }));
    });
    
    ws.on('message', (data) => {
      const response = JSON.parse(data);
      ws.close();
      
      if (response.result) {
        console.log('Bundle submitted successfully');
        console.log('Bundle Hash:', response.result.bundleHash);
        resolve(response.result.bundleHash);
      } else {
        console.error('Submission failed:', response.error);
        reject(response.error);
      }
    });
  });
}

arbitrage();
```
{% endcode %}



</details>

### Comparing Bundles and Private Transactions

Choose the appropriate method based on your use case:

| Aspect                | `bsc_privateTx`                    | `mev_sendBundle`                 |
| --------------------- | ---------------------------------- | -------------------------------- |
| Transaction count     | Single                             | Multiple                         |
| Atomicity             | Not applicable                     | All-or-nothing execution         |
| Use cases             | Simple transfers, individual swaps | Arbitrage, multi-step operations |
| Adding priority fees  | Via Multicall3                     | Separate transaction in bundle   |
| `mev_builders` format | Array: `["all"]`                   | Object: `{"all": ""}`            |

### Best Practices

1\. Use Sequential Nonces

All transactions in a bundle must have consecutive nonces starting from your wallet's current nonce:

```javascript
const nonce = await provider.getTransactionCount(wallet.address);

// Transaction 1: nonce
// Transaction 2: nonce + 1
// Transaction 3: nonce + 2
```

2\. Include a Priority Fee

Adding a fee payment as the final transaction increases the likelihood of bundle inclusion:

```javascript
const feeTx = await wallet.signTransaction({
  nonce: lastNonce,
  to: '0x6374Ca2da5646C73Eb444aB99780495d61035f9b',
  value: ethers.parseEther('0.001'),
  gasPrice: ethers.parseUnits('3', 'gwei'),
  gasLimit: 21000,
  chainId: 56
});
```

### Choose an Appropriate blocks\_count Value

The `blocks_count` parameter determines how long your bundle remains valid. Shorter validity periods signal higher urgency to builders:

| Use Case                 | Recommended blocks\_count |
| ------------------------ | ------------------------- |
| Time-sensitive arbitrage | 2–3                       |
| Standard bundles         | 5                         |
| Less urgent operations   | 10–20                     |

## Simulate Before Submitting

Test your transaction logic before submitting a bundle by simulating each transaction:

```javascript
await provider.call({
  to: tx1.to,
  data: tx1.data,
  value: tx1.value
});
```

### Troubleshooting

| Problem                | Solution                                                                                                                                                                                                                                                                                                                                                                                                                                                                                           |
| ---------------------- | -------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| Bundle Not Included    | <p>If your bundle is not included after the specified block count:</p><ul><li><strong>Increase the priority fee</strong> — Add a higher fee payment transaction</li><li><strong>Verify nonce sequence</strong> — Nonces must be strictly consecutive with no gaps</li><li><strong>Reduce blocks_count</strong> — Shorter validity signals higher priority to builders</li><li><strong>Validate each transaction</strong> — Ensure every transaction in the bundle is valid independently</li></ul> |
| "Invalid bundle" Error | <p></p><p>This error indicates a formatting or validation issue:</p><ul><li><strong>Check parameter format</strong> — The <code>transactions</code> parameter must be an array; <code>mev_builders</code> must be an object</li><li><strong>Verify signatures</strong> — All transactions must be properly signed</li><li><strong>Confirm chain ID</strong> — Use chain ID 56 for BSC Mainnet</li></ul>                                                                                            |
| Partial Execution      | <p></p><p>Bundles execute atomically, so partial execution should not occur. If you observe partial execution:</p><ul><li><strong>Contact support</strong> — This indicates a potential builder issue</li><li><strong>Verify using the bundle hash</strong> — Check the bundle status on a block explorer</li></ul>                                                                                                                                                                                |

