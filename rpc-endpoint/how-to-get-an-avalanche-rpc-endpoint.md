---
description: Step-by-step guide to getting a fast, reliable Avalanche RPC endpoint
---

# How to Get an Avalanche RPC Endpoint

Avalanche's C-Chain provides EVM-compatible smart contract execution with sub-second finality. Whether you're building DeFi protocols, NFT platforms, or gaming applications on Avalanche, this guide gets you connected in minutes.

### Features

* **C-Chain** — full EVM compatibility
* Archive data access (all plans)
* Trace & Debug methods (Starter+)
* WebSocket support
* Multi-region (Frankfurt, New York, Singapore)
* Dedicated Nodes available

### Step-by-Step: Get Your Avalanche RPC Endpoint

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

<figure><img src="../.gitbook/assets/image (1) (1) (1).png" alt=""><figcaption></figcaption></figure>

3.  Select:

    * **Protocol:** Avalanche
    * **Network:** Mainnet or Testnet
    * **API Interface:** JSON-RPC or Websocket
    * **Region:** Frankfurt (EU)

    <figure><img src="../.gitbook/assets/image.png" alt=""><figcaption></figcaption></figure>
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
   "result": "0xa86a" (Chain ID 43114)
}
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
console.log("Block:", await provider.getBlockNumber());
```
{% endtab %}

{% tab title="Python" %}
```python
from web3 import Web3
w3 = Web3(Web3.HTTPProvider("https://go.getblock.io/<YOUR-ACCESS-TOKEN>/"))
print("Chain ID:", w3.eth.chain_id)  # 43114
```
{% endtab %}

{% tab title="Viem" %}
```typescript
import { createPublicClient, http } from "viem";
import { arbitrum } from "viem/chains";

const client = createPublicClient({
  chain: avalanche,
  transport: http("https://go.getblock.io/<YOUR-ACCESS-TOKEN>/"),
});
```
{% endtab %}
{% endtabs %}

### Plans

Free plan: 50K CU/day. [Compare plans →](https://getblock.io/pricing/)

* [Full Avalanche API Reference →](https://docs.getblock.io/api-reference/avalanche-avax)
* [Dedicated Nodes →](https://getblock.io/dedicated-nodes/)

_Building on Avalanche?_ [_Contact us_](https://getblock.io/contact/) _for custom infrastructure._
