---
description: Step-by-step guide to getting a fast, reliable Base RPC endpoint
---

# How to Get a Base RPC Endpoint

Base has become one of the most active Ethereum Layer 2 networks for DeFi, social apps, and onchain consumer products.&#x20;

If you're building on Base for the first time or scaling a production application, this guide gets you set up with a reliable RPC endpoint in minutes.

### Step-by-Step: Get Your Base RPC Endpoint

{% stepper %}
{% step %}
#### Create a GetBlock Account

Go to [GetBlock Dashboard](https://account.getblock.io) and sign up. You can register with email or via Google/GitHub OAuth.&#x20;
{% endstep %}

{% step %}
#### Create an Avalanche Endpoint

Once logged in:

1. Click **"Shared Nodes"** in the left sidebar
2. Click **"Create New Endpoint"** or the **"+"** button

<figure><img src="../.gitbook/assets/image (1) (1) (1) (1).png" alt=""><figcaption></figcaption></figure>

3.  Select:

    * **Protocol:** Base
    * **Network:** Mainnet or Sepolia
    * **API Interface:** JSON-RPC or Websocket or MEV-Protected(JSON-RPC) or MEV-protected(Websocket)
    * **Region:** Frankfurt (EU) or Singapore or New York

    <figure><img src="../.gitbook/assets/image (2) (1).png" alt=""><figcaption></figcaption></figure>
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
The long string after `go.getblock.io/` is your **access token,** keep it private.
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
  "result": "0x2105"
}
```
{% endcode %}
{% endtab %}
{% endtabs %}
{% endstep %}
{% endstepper %}

### Code Examples

{% tabs %}
{% tab title="Ethers.js" %}
{% code overflow="wrap" %}
```javascript
import { JsonRpcProvider, formatEther } from "ethers";

const provider = new JsonRpcProvider(
  "https://go.getblock.io/<YOUR-ACCESS-TOKEN>/"
);

const blockNumber = await provider.getBlockNumber();
console.log("Latest Base block:", blockNumber);

const balance = await provider.getBalance("0xYOUR_ADDRESS");
console.log(`Balance: ${formatEther(balance)} ETH`);
```
{% endcode %}
{% endtab %}

{% tab title="Python(Web3.py)" %}
{% code overflow="wrap" %}
```python
from web3 import Web3

w3 = Web3(Web3.HTTPProvider("https://go.getblock.io/<YOUR-ACCESS-TOKEN>/"))

print("Chain ID:", w3.eth.chain_id)  # 8453
print("Latest block:", w3.eth.block_number)
```
{% endcode %}
{% endtab %}

{% tab title="Viem(Typescript)" %}
{% code overflow="wrap" %}
```typescript
import { createPublicClient, http } from "viem";
import { base } from "viem/chains";

const client = createPublicClient({
  chain: base,
  transport: http("https://go.getblock.io/<YOUR-ACCESS-TOKEN>/"),
});

const blockNumber = await client.getBlockNumber();
console.log("Block:", blockNumber);

const balance = await client.getBalance({
  address: "0xYOUR_ADDRESS",
});
console.log("Balance:", balance);
```
{% endcode %}
{% endtab %}
{% endtabs %}

### How to Connect MetaMask to Base via GetBlock

1. MetaMask → **Add Network** → **Add manually**
2. Enter:
   * **Network Name:** Base Mainnet (GetBlock)
   * **RPC URL:** `https://go.getblock.io/<YOUR-ACCESS-TOKEN>/`
   * **Chain ID:** `8453`
   * **Symbol:** `ETH`
   * **Explorer:** `https://basescan.org`
3. Save

**Base Sepolia Testnet:**

* Chain ID: `84532`
* Explorer: `https://sepolia.basescan.org`

### Why a Dedicated RPC Provider For Base?

Base inherits Ethereum's security while offering:

* **Low gas fees** (\~$0.01 per transaction)
* **EVM compatibility** — deploy existing Solidity contracts without changes
* **Coinbase ecosystem** — seamless fiat on-ramp integration
* **Growing DeFi ecosystem** — Aerodrome, Uniswap, Aave, and more

But Base's popularity means public RPC endpoints are frequently congested. A dedicated provider like GetBlock gives you consistent performance with up to 500 RPS and 99.9%+ uptime.

### Base Flashblocks Support

Base introduced **Flashblocks** — a mechanism for pre-confirmation of transactions at sub-second speeds. GetBlock supports Flashblocks, enabling your application to react to transactions inclusions faster than waiting for full block confirmation.

[How to Build a Base Flashblocks Listener →](https://docs.getblock.io/guides/how-to-build-a-base-flashblocks-listener)

### Choosing Your Plan

| Use Case                 | Plan                           | Why                                 |
| ------------------------ | ------------------------------ | ----------------------------------- |
| Building on Base         | **Free** ($0)                  | 50K CU/day, perfect for development |
| Production dApp          | **Starter** ($49/mo)           | 50M CU/month, 100 RPS               |
| High-traffic DeFi app    | **Advanced** ($199/mo)         | 220M CU/month, 300 RPS              |
| Onchain consumer product | **Pro** ($499/mo)              | 600M CU/month, 24/7 support         |
| Infrastructure provider  | **Dedicated** (from $1,000/mo) | Unlimited, custom SLA               |

### What's Next?

* [Full Base API Reference →](https://docs.getblock.io/api-reference/base)
* [How to Build a Base Flashblocks Listener →](https://docs.getblock.io/guides/how-to-build-a-base-flashblocks-listener)
* [Using Ethers.js with GetBlock →](https://docs.getblock.io/guides/using-web3-libraries/ethers.js-integration)
* [Configure a Dedicated Base Node →](https://getblock.io/dedicated-nodes/)
* Get testnet ETH from [Base Sepolia faucets](https://getblock.io/faucet/).

_Scaling on Base?_ [_Contact us_](https://getblock.io/contact/) _for dedicated infrastructure with custom performance tuning._
