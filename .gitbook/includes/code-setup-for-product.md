---
title: code setup for product
---

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
import Websocket from 'ws';
import ethers from 'ethers';

const SIDECAR_URL = 'ws://localhost:28334/ws';
const PRIVATE_KEY = 'your-private-key';
const RPC_URL = 'https://bsc-dataseed.binance.org';

// Constants
const MULTICALL3 = '0xcA11bde05977b3631167028862bE2a173976CA11';
const BLOXROUTE_FEE = '0x6374Ca2da5646C73Eb444aB99780495d61035f9b';

const MULTICALL_ABI = [
  'function aggregate3Value(tuple(address target, bool allowFailure, uint256 value, bytes callData)[] calls) payable returns (tuple(bool success, bytes returnData)[])'
];

async function sendPrivateTxWithTip() {
  const provider = new ethers.JsonRpcProvider(RPC_URL);
  const wallet = new ethers.Wallet(PRIVATE_KEY, provider);
  
  console.log('Wallet:', wallet.address);
  
  // Define amounts
  const tipAmount = ethers.parseEther('0.0001');      // 0.0001 BNB tip
  const transferAmount = ethers.parseEther('0.001');  // Your actual transfer
  const totalValue = tipAmount + transferAmount;
  
  // Build Multicall3 calls
  const calls = [
    {
      target: BLOXROUTE_FEE,      // Tip payment FIRST
      allowFailure: false,
      value: tipAmount,
      callData: '0x'              // Empty = simple transfer
    },
    {
      target: '0xYOUR_RECIPIENT', // Your actual transaction
      allowFailure: false,
      value: transferAmount,
      callData: '0x'              // Or your contract calldata
    }
  ];
  
  // Encode Multicall3 call
  const multicall = new ethers.Contract(MULTICALL3, MULTICALL_ABI, wallet);
  const data = multicall.interface.encodeFunctionData('aggregate3Value', [calls]);
  
  // Get nonce
  const nonce = await provider.getTransactionCount(wallet.address);
  
  // Sign transaction
  const signedTx = await wallet.signTransaction({
    nonce: nonce,
    to: MULTICALL3,
    value: totalValue,
    data: data,
    gasPrice: ethers.parseUnits('3', 'gwei'),
    gasLimit: 150000,  // Higher for Multicall3
    chainId: 56
  });
  
  console.log('Transaction built:');
  console.log('  Tip:', ethers.formatEther(tipAmount), 'BNB');
  console.log('  Transfer:', ethers.formatEther(transferAmount), 'BNB');
  console.log('  Total:', ethers.formatEther(totalValue), 'BNB');
  
  // Send via bsc_private_tx
  const ws = new WebSocket(SIDECAR_URL);
  
  ws.on('open', () => {
    const request = {
      jsonrpc: '2.0',
      id: 1,
      method: 'bsc_private_tx',
      params: {
        transaction: signedTx.slice(2),
        mev_builders: ['all']
      }
    };
    
    console.log('\nSending private TX with tip...');
    ws.send(JSON.stringify(request));
  });
  
  ws.on('message', (data) => {
    const response = JSON.parse(data);
    
    if (response.result) {
      console.log('\n✅ Transaction submitted!');
      console.log('TX Hash:', response.result.txHash);
      console.log('BSCScan:', `https://bscscan.com/tx/${response.result.txHash}`);
    } else {
      console.error('\n❌ Error:', response.error);
    }
    
    ws.close();
  });
}

sendPrivateTxWithTip().catch(console.error);
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
//
```
{% endstep %}
{% endstepper %}
