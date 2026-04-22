---
description: Step-by-step guide to getting a fast, reliable Arbitrum RPC endpoint
---

# How to Get an Arbitrum RPC Endpoint

Arbitrum One is the leading Ethereum L2 by TVL, hosting major DeFi protocols like GMX, Aave, Uniswap, and Camelot. If you're building on Arbitrum, you need a reliable RPC endpoint that can handle the network's high throughput without rate-limiting your application.

### Features

* **EVM-compatible** — use the same tools as Ethereum (ethers.js, web3.js, Hardhat, Foundry)
* **Archive data** — full historical state queries
* **Trace & Debug** — `debug_traceTransaction` and related methods (Starter+)
* **WebSocket** — `wss://go.getblock.io/<TOKEN>/` for real-time subscriptions
* **Multi-region** — Frankfurt, New York, Singapore

{% hint style="info" %}
Arbitrum mainnet Dedicated Nodes may have custom pricing due to higher infrastructure requirements. [Contact sales](https://getblock.io/contact/) for details.
{% endhint %}

### Step-by-Step: Get Your Arbitrum RPC Endpoint

{% stepper %}
{% step %}
#### Create a GetBlock Account

Go to [GetBlock Dashboard](https://account.getblock.io) and sign up. You can register with email or via Google/GitHub OAuth.&#x20;
{% endstep %}

{% step %}
#### Create a Arbitrum Endpoint

Once logged in:

1. Click **"Shared Nodes"** in the left sidebar
2. Click **"Create New Endpoint"** or the **"+"** button

<figure><img src="../.gitbook/assets/image (1) (1) (1).png" alt=""><figcaption></figcaption></figure>

3.  Select:

    * **Protocol:** Arbitrum
    * **Network:** Mainnet or Sepolia
    * **API Interface:** JSON-RPC or Websocket
    * **Region:** Frankfurt (EU) or New York, USA

    <figure><img src="../.gitbook/assets/image (1).png" alt=""><figcaption></figcaption></figure>
4. Click **"Create":** Your endpoint URL will be generated immediately.
{% endstep %}

{% step %}
#### Copy Your Endpoint URL

Your endpoint URL looks like this:

```
https://go.getblock.io/a1b2c3d4e5f6789012345678abcdef01/
```

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
  -d '{"jsonrpc":"2.0","method":"eth_chainId","params":[],"id":1}'
```
{% endcode %}
{% endtab %}

{% tab title="Response" %}
{% code overflow="wrap" %}
```json
"result": "0xa4b1" 
```
{% endcode %}
{% endtab %}
{% endtabs %}
{% endstep %}
{% endstepper %}

#### Code Sample

{% tabs %}
{% tab title="Ethers.js" %}
```javascript
import { JsonRpcProvider } from "ethers";

const provider = new JsonRpcProvider("https://go.getblock.io/<YOUR-ACCESS-TOKEN>/");
const block = await provider.getBlockNumber();
console.log("Latest Arbitrum block:", block);
```
{% endtab %}

{% tab title="Python" %}
```python
from web3 import Web3

w3 = Web3(Web3.HTTPProvider("https://go.getblock.io/<YOUR-ACCESS-TOKEN>/"))
print("Chain ID:", w3.eth.chain_id)  # 42161
```
{% endtab %}

{% tab title="Viem" %}
```typescript
import { createPublicClient, http } from "viem";
import { arbitrum } from "viem/chains";

const client = createPublicClient({
  chain: arbitrum,
  transport: http("https://go.getblock.io/<YOUR-ACCESS-TOKEN>/"),
});
```
{% endtab %}
{% endtabs %}

### Arbitrum Testnets

| Network          | Chain ID | Purpose               |
| ---------------- | -------- | --------------------- |
| Arbitrum Sepolia | 421614   | Development & testing |

> Get testnet ETH from [Arbitrum faucets](https://getblock.io/faucet/).

### Plans

| Use Case        | Plan                   | CU        | RPS       |
| --------------- | ---------------------- | --------- | --------- |
| Development     | **Free**               | 50K/day   | 20        |
| Production dApp | **Starter** ($49/mo)   | 50M/mo    | 100       |
| DeFi / DEX      | **Advanced** ($199/mo) | 220M/mo   | 300       |
| High-traffic    | **Pro** ($499/mo)      | 600M/mo   | 500       |
| Institutional   | **Dedicated** (custom) | Unlimited | Unlimited |

### What's Next?

* [Full Arbitrum API Reference →](https://docs.getblock.io/api-reference/arbitrum-arb)
* [How to Build a Hyperliquid Whale Tracker Bot →](https://docs.getblock.io/guides/how-to-build-a-hyperliquid-whale-tracker-bot-with-getblock)
* [Configure a Dedicated Arbitrum Node →](https://getblock.io/dedicated-nodes/)
* [Learn more about our pricing](https://getblock.io/pricing/)

_Building DeFi on Arbitrum?_ [_Talk to us_](https://getblock.io/contact/) _about enterprise-grade infrastructure._
