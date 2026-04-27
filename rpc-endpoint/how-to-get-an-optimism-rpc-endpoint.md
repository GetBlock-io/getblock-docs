---
description: Step-by-step guide to getting a fast, reliable Optimism RPC endpoint
---

# How to Get an Optimism RPC Endpoint

Optimism (OP Mainnet) is a leading Ethereum L2 and the foundation of the Superchain ecosystem. With sub-dollar transaction costs and full EVM compatibility, it hosts major protocols like Velodrome, Synthetix, and Aave. Here's how to get a reliable OP Mainnet RPC endpoint.

### Features

* Archive data access (all plans)
* Trace & Debug methods (Starter+)
* WebSocket support
* Multi-region (Frankfurt, New York, Singapore)
* Dedicated Nodes available

### Step-by-Step: Get Your Optimism RPC Endpoint

{% stepper %}
{% step %}
#### Create a GetBlock Account

Go to [GetBlock Dashboard](https://account.getblock.io) and sign up. You can register with email or via Google/GitHub OAuth.&#x20;
{% endstep %}

{% step %}
#### Create an Optimism Endpoint

Once logged in:

1. Click **"Shared Nodes"** in the left sidebar
2. Click **"Create New Endpoint"** or the **"+"** button

<figure><img src="../.gitbook/assets/image (1) (1) (1) (1).png" alt=""><figcaption></figcaption></figure>

3.  Select:

    * **Protocol:** Optimism
    * **Network:** Mainnet or Sepolia
    * **API Interface:** JSON-RPC or Websocket or GraphQL&#x20;
    * **Region:** Frankfurt (EU)

    <figure><img src="../.gitbook/assets/image (5).png" alt=""><figcaption></figcaption></figure>


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
    "result": "0xa"
 }
```
{% endcode %}
{% endtab %}
{% endtabs %}
{% endstep %}
{% endstepper %}

#### Code Sample

{% tabs %}
{% tab title="ethers.js" %}
{% code overflow="wrap" %}
```javascript
import { JsonRpcProvider } from "ethers";
const provider = new JsonRpcProvider("https://go.getblock.io/<YOUR-ACCESS-TOKEN>/");
console.log("Block:", await provider.getBlockNumber());
```
{% endcode %}
{% endtab %}

{% tab title="Viem" %}
{% code overflow="wrap" %}
```typescript
import { createPublicClient, http } from "viem";
import { optimism } from "viem/chains";
const client = createPublicClient({
  chain: optimism,
  transport: http("https://go.getblock.io/<YOUR-ACCESS-TOKEN>/"),
});
```
{% endcode %}
{% endtab %}
{% endtabs %}

### What's Next?

* [Full OP API Reference](https://docs.getblock.io/api-reference/optimism-op)
* [Dedicated Nodes](https://getblock.io/dedicated-nodes/)
* [Learn more about our pricing](https://getblock.io/pricing/)

_Need Superchain infrastructure?_ [_Contact us_](https://getblock.io/contact/)_._
