---
description: >-
  Switch from QuickNode to GetBlock for more chains, lower pricing, and
  transparent billing. Complete migration  guide with code examples.
---

# How to Migrate from QuickNode to GetBlock — Step-by-Step

QuickNode is a strong provider, but some teams find they need more chain coverage, a permanent free tier, or more predictable pricing. Here's how to migrate from QuickNode to GetBlock.This migration only takes 5–10 minutes.

## Why Teams Switch

| Factor              | QuickNode                                    | GetBlock                                   |
| ------------------- | -------------------------------------------- | ------------------------------------------ |
| Free tier           | ❌ Trial only (expires)                       | ✅ **Free forever** (50K CU/day)            |
| Chain coverage      | \~75                                         | **100+**                                   |
| Billing model       | Credits (method-weighted, harder to predict) | **CU tiers with clear pricing**            |
| Region selection    | Auto-routed                                  | **Choose: Frankfurt, New York, Singapore** |
| Solana HFT tools    | WebSocket, gRPC                              | **StreamFirst + LandFirst + TradeFirst**   |
| BSC private mempool | Not available                                | ✅ **BloXroute BDN integration**            |
| Annual discount     | Varies                                       | **20% off on all plans**                   |

### How to Switch From QuickNode to GetBlock

{% stepper %}
{% step %}
#### Create Your GetBlock Endpoint

1. Sign up at [account.getblock.io](https://account.getblock.io/)
2. Go to **Shared Nodes** → **Create New Endpoint**
3. Select your blockchain, network, and API interface
4. Choose your region (closest to your servers)
5. Copy the endpoint URL

**Infura format:**

```bash
https://your-endpoint-name.quiknode.pro/YOUR_TOKEN/
```

**GetBlock format:**

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
// Before (Infura)
const provider = new JsonRpcProvider(
  "https://xxx.quiknode.pro/YOUR_QN_TOKEN/"
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
  transport: http(
  "https://xxx.quiknode.pro/YOUR_QN_TOKEN/""
);
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
w3 = Web3(Web3.HTTPProvider("https://xxx.quiknode.pro/YOUR_QN_TOKEN/"))

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
    url: "https://xxx.quiknode.pro/YOUR_QN_TOKEN/"
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
mainnet = "https://xxx.quiknode.pro/YOUR_QN_TOKEN/"

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
const wsProvider = new WebSocketProvider("wss://xxx.quiknode.pro/YOUR_QN_TOKEN");

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

## Feature Mapping: QuickNode → GetBlock

| QuickNode Feature   | GetBlock Equivalent                                |
| ------------------- | -------------------------------------------------- |
| RPC Endpoints       | ✅ Shared Nodes                                     |
| WebSocket           | ✅ WebSocket support                                |
| Archive data        | ✅ All plans including Free                         |
| Streams             | ✅ WebSocket subscriptions + Tracker (webhooks)     |
| Marketplace add-ons | Partial — specialized tools built-in (BSC, Solana) |
| Team management     | ✅ Team accounts (up to 30 users)                   |
| Dedicated Nodes     | ✅ From $1,000/mo, unlimited                        |
| Debug/Trace         | ✅ Starter+ plans                                   |
| Multi-region        | ✅ 3 regions (explicit selection)                   |

### What You Gain

* **Permanent free tier** — no expiration, no trial
* **25+ more chains** — broader multi-chain coverage
* **Explicit region control** — choose your data center
* **Solana HFT infrastructure** — StreamFirst, LandFirst, TradeFirst
* **BSC private mempool** — BloXroute BDN with bundle support
* **Simpler, more predictable pricing**

### What You Might Miss

* **QuickNode Marketplace add-ons** — some niche tools may not have direct equivalents
* **QuickNode Streams** → Use GetBlock WebSocket + Tracker for real-time data
* **IPFS/NFT-specific APIs** → Use specialized third-party services

<a href="https://account.getblock.io" class="button primary">Ready to switch? and have your new endpoints running in under 5 minutes.</a>
