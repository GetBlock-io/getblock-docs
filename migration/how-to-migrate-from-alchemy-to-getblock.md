---
description: >-
  Switch from Alchemy to GetBlock in minutes. This migration guide covers
  endpoint URLs, SDK configuration, feature mapping, and why teams are 
  switching.
---

# How to Migrate from Alchemy to GetBlock

Switching RPC providers sounds harder than it is. In most cases, it's literally changing one URL in your codebase. This guide walks you through migrating from Alchemy to GetBlock, including differences in endpoint formats, SDK configuration changes, and feature mappings. The migration only takes 5–15 minutes for most projects.

## Why Teams Switch from Alchemy to GetBlock

| Reason                   | Details                                                      |
| ------------------------ | ------------------------------------------------------------ |
| **More chains**          | GetBlock supports 100+ blockchains vs Alchemy's \~70         |
| **Lower cost at scale**  | GetBlock Pro ($499/mo, 600M CU) vs comparable Alchemy tiers  |
| **Geographic control**   | Choose your endpoint region (Frankfurt, New York, Singapore) |
| **Archive on all plans** | Including the free tier — Alchemy requires paid plans        |
| **Solana HFT tools**     | StreamFirst, LandFirst, TradeFirst — no Alchemy equivalent   |
| **BSC private mempool**  | BloXroute BDN integration — not available on Alchemy         |
| **Simpler pricing**      | Predictable CU tiers vs Alchemy's compute unit complexity    |

### How to Switch From Alchemy To GetBlock

{% stepper %}
{% step %}
#### Create Your GetBlock Endpoint

1. Sign up at [account.getblock.io](https://account.getblock.io/)
2. Go to **Shared Nodes** → **Create New Endpoint**
3. Select your blockchain, network, and API interface
4. Choose your region (closest to your servers)
5. Copy the endpoint URL

**Alchemy endpoint format:**

```bash
https://eth-mainnet.g.alchemy.com/v2/YOUR_API_KEY
```

**GetBlock endpoint format:**

```bash
https://go.getblock.io/YOUR_ACCESS_TOKEN/
```
{% endstep %}

{% step %}
#### Update Your Code

{% tabs %}
{% tab title="ethers.js" %}
{% code overflow="wrap" %}
```javascript
// Before (Alchemy)
const provider = new JsonRpcProvider(
  "https://eth-mainnet.g.alchemy.com/v2/YOUR_ALCHEMY_KEY"
);

// After (GetBlock)
const provider = new JsonRpcProvider(
  "https://go.getblock.io/YOUR_GETBLOCK_TOKEN/"
);
```
{% endcode %}
{% endtab %}

{% tab title="Viem" %}
{% code overflow="wrap" %}
```typescript
// Before
const client = createPublicClient({
  chain: mainnet,
  transport: http("https://eth-mainnet.g.alchemy.com/v2/YOUR_ALCHEMY_KEY"),
});

// After
const client = createPublicClient({
  chain: mainnet,
  transport: http("https://go.getblock.io/YOUR_GETBLOCK_TOKEN/"),
});
```
{% endcode %}
{% endtab %}

{% tab title="web3.py" %}
{% code overflow="wrap" %}
```python
# Before
w3 = Web3(Web3.HTTPProvider("https://eth-mainnet.g.alchemy.com/v2/YOUR_ALCHEMY_KEY"))

# After
w3 = Web3(Web3.HTTPProvider("https://go.getblock.io/YOUR_GETBLOCK_TOKEN/"))
```
{% endcode %}
{% endtab %}

{% tab title="Hardhat" %}
{% code overflow="wrap" %}
```javascript
// hardhat.config.js — Before
networks: {
  mainnet: {
    url: "https://eth-mainnet.g.alchemy.com/v2/YOUR_ALCHEMY_KEY"
  }
}

// hardhat.config.js — After
networks: {
  mainnet: {
    url: "https://go.getblock.io/YOUR_GETBLOCK_TOKEN/"
  }
}
```
{% endcode %}
{% endtab %}

{% tab title="Foundry" %}
{% code overflow="wrap" %}
```toml
# foundry.toml — Before
[rpc_endpoints]
mainnet = "https://eth-mainnet.g.alchemy.com/v2/YOUR_ALCHEMY_KEY"

# foundry.toml — After
[rpc_endpoints]
mainnet = "https://go.getblock.io/YOUR_GETBLOCK_TOKEN/"
```
{% endcode %}
{% endtab %}

{% tab title="Wesocket" %}
{% code overflow="wrap" %}
```javascript
// Before
const wsProvider = new WebSocketProvider("wss://eth-mainnet.g.alchemy.com/v2/YOUR_ALCHEMY_KEY");

// After
const wsProvider = new WebSocketProvider("wss://go.getblock.io/YOUR_GETBLOCK_TOKEN/");
```
{% endcode %}
{% endtab %}
{% endtabs %}
{% endstep %}

{% step %}
#### Environment Variables (recommended)

Best practice: use environment variables to so you can switch providers without code changes.

```bash
# .env
RPC_URL=https://go.getblock.io/YOUR_GETBLOCK_TOKEN/
```

```javascript
const provider = new JsonRpcProvider(process.env.RPC_URL);
```
{% endstep %}

{% step %}
#### Verify Everything Works

Run a quick check:

{% code overflow="wrap" %}
```bash
curl -X POST $RPC_URL \
  -H "Content-Type: application/json" \
  -d '{"jsonrpc":"2.0","method":"eth_blockNumber","params":[],"id":1}'
```
{% endcode %}

If you get a valid response with a block number, you're good.
{% endstep %}
{% endstepper %}

## Feature Mapping: Alchemy → GetBlock

| Alchemy Feature     | GetBlock Equivalent  | Notes                                           |
| ------------------- | -------------------- | ----------------------------------------------- |
| RPC Endpoints       | ✅ Shared Nodes       | Same JSON-RPC methods                           |
| WebSocket           | ✅ WebSocket support  | Same subscription methods                       |
| Archive data        | ✅ Archive mode       | Available on all plans (Alchemy requires paid)  |
| Debug/Trace         | ✅ Trace & Debug      | Available on Starter+                           |
| NFT API             | ❌ Not available      | Use third-party NFT APIs                        |
| Token API           | ❌ Not available      | Use standard RPC methods or third-party         |
| Notify (webhooks)   | ✅ Blockchain Tracker | GetBlock's webhook solution                     |
| Enhanced APIs       | Standard RPC         | GetBlock focuses on RPC infrastructure          |
| Alchemy SDK         | Standard libraries   | Use ethers.js, web3.js directly                 |
| Dashboard analytics | ✅ Statistics         | Method tracking, CU monitoring                  |
| Multi-chain         | ✅ 100+ chains        | More chains than Alchemy (\~70)                 |
| Regional selection  | ✅ 3 regions          | Alchemy doesn't offer explicit region selection |
| Dedicated Nodes     | ✅ Dedicated Nodes    | From $1,000/mo, unlimited requests              |
| Team accounts       | ✅ Team accounts      | Up to 30 users, role-based access               |

### What You'll Gain

* More blockchain coverage (100+ vs \~70)
* Archive data on the free plan
* Control over endpoint geography
* More affordable dedicated nodes
* Solana HFT tools (StreamFirst, LandFirst, TradeFirst)
* BSC private mempool access

### What You'll Need Alternatives For

* **Alchemy NFT API** → Use Simplehash, Reservoir, or direct RPC calls
* **Alchemy Token API** → Use standard `eth_call` for ERC-20 balances, or Covalent/Moralis
* **Alchemy Notify** → GetBlock Tracker for webhook-style notifications
* **Alchemy SDK** → Standard ethers.js/web3.js (works identically with GetBlock)

## Run Both Providers in Parallel (Optional)

For mission-critical applications, consider running both providers during migration:

{% code overflow="wrap" %}
```javascript
import { FallbackProvider, JsonRpcProvider } from "ethers";

const getblock = new JsonRpcProvider("https://go.getblock.io/YOUR_TOKEN/");
const alchemy = new JsonRpcProvider("https://eth-mainnet.g.alchemy.com/v2/YOUR_KEY");

// Primary: GetBlock, Fallback: Alchemy
const provider = new FallbackProvider([
  { provider: getblock, priority: 1, weight: 1 },
  { provider: alchemy, priority: 2, weight: 1 },
]);
```
{% endcode %}

Once you've verified GetBlock is stable for your workload (typically 1–2 weeks), remove the fallback.

## Common Questions

<details>

<summary>Will my existing code break?</summary>

No. GetBlock supports all standard Ethereum JSON-RPC methods. If it works with Alchemy's RPC, it works with GetBlock.

</details>

<details>

<summary>Do I need to change anything besides the URL?</summary>

For standard RPC usage, no. If you're using Alchemy-specific SDKs or APIs (NFT API, Token API, Notify), you'll need alternatives for those specific features.

</details>

<details>

<summary>Can I migrate multiple chains at once?</summary>

Yes. Create endpoints for each chain in your GetBlock dashboard and update the URLs in your code. Each chain gets its own access token.

</details>

<details>

<summary>How do I migrate a Dedicated Node from Alchemy?</summary>

[Contact GetBlock sales](https://getblock.io/contact/) to configure a Dedicated Node matching your current Alchemy setup. GetBlock Dedicated Nodes start at $1,000/month with unlimited requests.

</details>

_Ready to switch?_ <a href="https://account.getblock.io" class="button primary"> Create your free GetBlock account</a>_and have your new endpoints running in under 5 minutes._
