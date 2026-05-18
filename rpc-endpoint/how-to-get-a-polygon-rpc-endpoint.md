---
description: Step-by-step guide to getting a fast, reliable Polygon RPC endpoint.
---

# How to Get a Polygon RPC Endpoint

Polygon PoS is one of the most widely used Ethereum scaling solutions, powering DeFi protocols, NFT marketplaces, gaming applications, and enterprise solutions.&#x20;

### Available Service on GetBlock

* **Archive data:** query historical state at any block (all plans)
* **Trace & Debug:** `debug_traceTransaction`, `trace_block` (Starter+)
* **WebSocket: r**eal-time subscriptions via `wss://go.getblock.io/<TOKEN>/`
* **Multi-region:** Frankfurt, New York, Singapore
* **Dedicated Nodes:** unlimited RPS, from $1,000/mo

This guide shows you how to get a reliable Polygon RPC endpoint — from free development access to production-grade infrastructure.

### Step-by-Step: Get Your Polygon RPC Endpoint

{% stepper %}
{% step %}
#### Create a GetBlock Account

Go to [GetBlock Dashboard](https://account.getblock.io) and sign up. You can register with email or via Google/GitHub OAuth.&#x20;
{% endstep %}

{% step %}
#### Create a Polygon Endpoint

Once logged in:

1. Click **"Shared Nodes"** in the left sidebar
2. Click **"Create New Endpoint"** or the **"+"** button

<figure><img src="../.gitbook/assets/image (1) (1) (1) (1) (1).png" alt=""><figcaption></figcaption></figure>

3.  Select:

    * **Protocol:** Polygon
    * **Network:** Mainnet or Amoy(test network)
    * **API Interface:** JSON-RPC or WebSocket
    * **Region:** Frankfurt (EU), New York (US), or Singapore (APAC)

    <figure><img src="../.gitbook/assets/image (8).png" alt=""><figcaption></figcaption></figure>
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
{% endtab %}

{% tab title="Response" %}
```json
{
  "jsonrpc": "2.0",
  "result": 0x89,
  "id": 1
}
```
{% endtab %}
{% endtabs %}
{% endstep %}
{% endstepper %}

### Code Example

{% tabs %}
{% tab title="Ethers.js" %}
```javascript
import { JsonRpcProvider } from "ethers";

const provider = new JsonRpcProvider("https://go.getblock.io/<YOUR-ACCESS-TOKEN>/");
const block = await provider.getBlockNumber();
console.log("Latest Polygon block:", block);
```
{% endtab %}

{% tab title="Python" %}
```python
from web3 import Web3

w3 = Web3(Web3.HTTPProvider("https://go.getblock.io/<YOUR-ACCESS-TOKEN>/"))
print("Chain ID:", w3.eth.chain_id)  # 137
print("Block:", w3.eth.block_number)
```
{% endtab %}
{% endtabs %}

### Plans

| Use Case     | Plan                    | CU        | RPS       |
| ------------ | ----------------------- | --------- | --------- |
| Learning     | **Free**                | 50K/day   | 20        |
| Side project | **Starter** ($49/mo)    | 50M/mo    | 100       |
| Production   | **Advanced** ($199/mo)  | 220M/mo   | 300       |
| High-traffic | **Pro** ($499/mo)       | 600M/mo   | 500       |
| Enterprise   | **Dedicated** ($1K+/mo) | Unlimited | Unlimited |

[You can learn more about our pricing here](https://getblock.io/pricing/)

### What's Next?

* [Full Polygon API Reference](https://docs.getblock.io/api-reference/polygon-matic)
* [Using Web3.js with GetBlock ](https://docs.getblock.io/guides/using-web3-libraries/web3.js-integration)
* [Configure a Dedicated Polygon Node ](https://getblock.io/dedicated-nodes/)

_Need enterprise Polygon infrastructure?_ [_Contact us_](https://getblock.io/contact/)_._
