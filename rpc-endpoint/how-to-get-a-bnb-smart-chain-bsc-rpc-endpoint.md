---
description: Step-by-step guide to getting a fast, reliableBSC RPC endpoint
---

# How to Get a BNB Smart Chain (BSC) RPC Endpoint

BNB Smart Chain remains one of the most active blockchains for DeFi, trading bots, and dApp development. With block times under 3 seconds and significantly lower gas fees than Ethereum, BSC handles massive transaction volumes, and your RPC infrastructure needs to keep pace.&#x20;

This guide walks you through setting up BSC RPC access with GetBlock, from free development endpoints to Accelerated Dedicated Nodes with private mempool support.

### Step-by-Step: Get Your BSC RPC Endpoint

{% stepper %}
{% step %}
#### Create a GetBlock Account

Go to [GetBlock Dashboard](https://account.getblock.io) and sign up. You can register with email or via Google/GitHub OAuth.&#x20;
{% endstep %}

{% step %}
#### Create a BSC Endpoint

Once logged in:

1. Click **"Shared Nodes"** in the left sidebar
2. Click **"Create New Endpoint"** or the **"+"** button

<figure><img src="../.gitbook/assets/image (1) (1) (1) (1) (1).png" alt=""><figcaption></figcaption></figure>

3.  Select:

    * **Protocol:** BSC
    * **Network:** Mainnet or Testnet
    * **API Interface:** JSON-RPC or Websocket or GraphQL or MEV-Protected(JSON-RPC) or MEV-Protected(Websocket)
    * **Region:** Frankfurt (EU) or Singapore or New York

    <figure><img src="../.gitbook/assets/image (3) (1).png" alt=""><figcaption></figcaption></figure>
4. Click **"Create":** Your endpoint URL will be generated immediately.
{% endstep %}

{% step %}
#### Copy Your Endpoint URL

Your endpoint URL looks like this:

{% code overflow="wrap" %}
```bash
https://go.getblock.io/a1b2c3d4e5f6789012345678abcdef01/
```
{% endcode %}

{% hint style="warning" %}
The long string after `go.getblock.io/` is your **access token** — keep it private.
{% endhint %}
{% endstep %}

{% step %}
#### Test the Connection

{% tabs %}
{% tab title="cURL" %}
{% code overflow="wrap" %}
```bash
curl -X POST https://go.getblock.io/<YOUR-ACCESS-TOKEN>/ \
  -H "Content-Type: application/json" \
  -d '{
  "jsonrpc":"2.0",
  "method":"eth_chainId",
  "params":[],
  "id":1
  }'
```
{% endcode %}
{% endtab %}

{% tab title="Response" %}
{% code overflow="wrap" %}
```bash
{
   "jsonrpc":"2.0",
    "id":1,
    "result":"0x38"
 }
```
{% endcode %}
{% endtab %}
{% endtabs %}
{% endstep %}
{% endstepper %}

### Code Examples

{% tabs %}
{% tab title="cURL — Get BEP-20 Token Balance" %}
{% code overflow="wrap" %}
```bash
# Get USDT balance on BSC using eth_call
curl -X POST https://go.getblock.io/<YOUR-ACCESS-TOKEN>/ \
  -H "Content-Type: application/json" \
  -d '{
    "jsonrpc": "2.0",
    "method": "eth_call",
    "params": [{
      "to": "0x55d398326f99059fF775485246999027B3197955",
      "data": "0x70a08231000000000000000000000000YOUR_ADDRESS_WITHOUT_0x"
    }, "latest"],
    "id": "getblock.io"
  }'
```
{% endcode %}
{% endtab %}

{% tab title="ethers.js" %}
{% code overflow="wrap" %}
```javascript
import { JsonRpcProvider, formatEther } from "ethers";

const provider = new JsonRpcProvider(
  "https://go.getblock.io/<YOUR-ACCESS-TOKEN>/"
);

// Get BNB balance
const balance = await provider.getBalance("0x8894E0a0c962CB723c1ef41B18a7bb7E76FB4675");
console.log(`Balance: ${formatEther(balance)} BNB`);

// Get gas price
const gasPrice = await provider.getFeeData();
console.log(`Gas price: ${gasPrice.gasPrice} wei`);

// Read a BEP-20 token balance (e.g., USDT on BSC)
const usdtContract = new Contract(
  "0x55d398326f99059fF775485246999027B3197955", // BSC USDT
  ["function balanceOf(address) view returns (uint256)"],
  provider
);
const tokenBalance = await usdtContract.balanceOf("0x...");
```
{% endcode %}
{% endtab %}

{% tab title="Python (web3.py)" %}
{% code overflow="wrap" %}
```python
from web3 import Web3

w3 = Web3(Web3.HTTPProvider("https://go.getblock.io/<YOUR-ACCESS-TOKEN>/"))

print("Connected:", w3.is_connected())
print("Chain ID:", w3.eth.chain_id)  # 56 for BSC Mainnet
print("Latest block:", w3.eth.block_number)

# Get BNB balance
balance = w3.eth.get_balance("0x8894E0a0c962CB723c1ef41B18a7bb7E76FB4675")
print(f"Balance: {w3.from_wei(balance, 'ether')} BNB")
```
{% endcode %}
{% endtab %}
{% endtabs %}

### Connect MetaMask to BSC via GetBlock

1. Open MetaMask → **Add Network** → **Add manually**
2. Enter:
   * **Network Name:** BSC Mainnet (GetBlock)
   * **RPC URL:** `https://go.getblock.io/<YOUR-ACCESS-TOKEN>/`
   * **Chain ID:** `56`
   * **Symbol:** `BNB`
   * **Explorer:** `https://bscscan.com`
3. Save

### Public BSC RPC vs GetBlock

| Feature             | Public RPC       | GetBlock                          |
| ------------------- | ---------------- | --------------------------------- |
| Rate limits         | Heavy throttling | Up to 500 RPS                     |
| Uptime              | No guarantee     | 99.9%+ SLA                        |
| Trace/Debug methods | Not available    | Available (Starter+)              |
| Archive data        | Limited          | Full history                      |
| Private mempool     | No               | Available (Accelerated Dedicated) |
| MEV protection      | None             | Via BloXroute BDN integration     |
| WebSocket           | Unreliable       | Persistent connections            |

### BSC Advanced Tooling — Exclusive to GetBlock

#### Accelerated Dedicated Node

GetBlock offers BSC-specific infrastructure with an integrated BloXroute BDN (Blockchain Distribution Network):

**Private Mempool Submission**

Submit transactions to a private mempool, bypassing the public mempool entirely:

* **MEV protection** — your transactions aren't visible to sandwich bots
* **Higher landing rate** — direct submission to block producers
* **Priority fee support** — set custom priority fees for faster inclusion

[How to Submit to Private Mempool](https://docs.getblock.io/bsc-advanced-tooling/bsc-accelerated-dedicated-node/how-to-submit-transaction-to-private-mempool)

**Bundle Support (Multicall3)**

Execute multiple transactions atomically:

* All-or-nothing execution
* MEV-resistant operations
* Complex DeFi strategies in a single block

[How to Use Bundles](https://docs.getblock.io/bsc-advanced-tooling/bsc-accelerated-dedicated-node/sending-transactions-to-private-mempool-priority-fee/how-to-use-bundle)

**Stream Subscription**

Subscribe to real-time BSC data streams:

* New pending transactions
* New blocks
* Transaction receipts

[How to Subscribe to Streams ](https://docs.getblock.io/bsc-advanced-tooling/bsc-chain-accelerated-dedicated-node/how-to-subscribe-to-stream)

### Choosing Your Plan

| Use Case              | Plan                               | Key Benefit                |
| --------------------- | ---------------------------------- | -------------------------- |
| Development & testing | **Free**                           | 50K CU/day                 |
| Production dApp       | **Starter** ($49/mo)               | 100 RPS, trace/debug       |
| DeFi protocol         | **Advanced** ($199/mo)             | 300 RPS, archive           |
| Trading bot / DEX     | **Dedicated** (from $1,000/mo)     | Unlimited, private mempool |
| HFT / MEV             | **Accelerated Dedicated** (custom) | BloXroute BDN, bundles     |

### What's Next?

* [Full BSC API Reference](https://docs.getblock.io/api-reference/bnb-smart-chain-bsc)
* [BSC Private Mempool Guide](https://docs.getblock.io/bsc-advanced-tooling/bsc-accelerated-dedicated-node/how-to-submit-transaction-to-private-mempool)
* [Configure a Dedicated BSC Node](https://getblock.io/dedicated-nodes/)
* [Using Web3.js with GetBlock](https://docs.getblock.io/guides/using-web3-libraries/web3.js-integration)
* [Configure Dedicated Node](https://getblock.io/dedicated-nodes/)
* [Learn more about our pricing](https://getblock.io/pricing/)

_Need high-performance BSC infrastructure for trading or DeFi?_ [_Contact us_](https://getblock.io/contact/) _to discuss Accelerated Dedicated Nodes with private mempool and bundle support._
