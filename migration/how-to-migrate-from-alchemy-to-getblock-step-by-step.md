---
description: >-
  Switch from Alchemy to GetBlock in minutes. This migration guide covers
  endpoint URLs, SDK configuration, feature mapping, and why teams are 
  switching.
---

# How to Migrate from Alchemy to GetBlock — Step-by-Step

Switching RPC providers sounds harder than it is. In most cases, it's literally changing one URL in your codebase. This guide walks you through migrating from Alchemy to GetBlock, including differences in endpoint formats, SDK configuration changes, and feature mappings. The migration only takes 5–15 minutes for most projects.

## Why Teams Switch from Alchemy to GetBlock

<table data-search="false"><thead><tr><th>Reason</th><th>Details</th></tr></thead><tbody><tr><td><strong>More chains</strong></td><td>GetBlock supports 130+ blockchains vs Alchemy's ~70</td></tr><tr><td><strong>Lower cost at scale</strong></td><td>GetBlock Pro ($499/mo, 1B CU) vs comparable Alchemy tiers</td></tr><tr><td><strong>Geographic control</strong></td><td>Choose your endpoint region (Frankfurt, New York, Singapore)</td></tr><tr><td><strong>Archive on all plans</strong></td><td>Including the free tier — Alchemy requires paid plans</td></tr><tr><td><strong>Solana HFT tools</strong></td><td>StreamFirst, LandFirst, TradeFirst — no Alchemy equivalent</td></tr><tr><td><strong>BSC private mempool</strong></td><td>BloXroute BDN integration — not available on Alchemy</td></tr><tr><td><strong>Simpler pricing</strong></td><td>Predictable CU tiers vs Alchemy's compute unit complexity</td></tr></tbody></table>

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
https://shared.eu-central-1.getblock.io/YOUR_ACCESS_TOKEN/
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
  "https://shared.eu-central-1.getblock.io/YOUR_GETBLOCK_TOKEN/"
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
  transport: http("https://shared.eu-central-1.getblock.io/YOUR_GETBLOCK_TOKEN/"),
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
w3 = Web3(Web3.HTTPProvider("https://shared.eu-central-1.getblock.io/YOUR_GETBLOCK_TOKEN/"))
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
    url: "https://shared.eu-central-1.getblock.io/YOUR_GETBLOCK_TOKEN/"
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
mainnet = "https://shared.eu-central-1.getblock.io/YOUR_GETBLOCK_TOKEN/"
```
{% endcode %}
{% endtab %}

{% tab title="Websocket" %}
{% code overflow="wrap" %}
```javascript
// Before
const wsProvider = new WebSocketProvider("wss://eth-mainnet.g.alchemy.com/v2/YOUR_ALCHEMY_KEY");

// After
const wsProvider = new WebSocketProvider("wss://shared.eu-central-1.getblock.io/YOUR_GETBLOCK_TOKEN/");
```
{% endcode %}
{% endtab %}
{% endtabs %}
{% endstep %}

{% step %}
#### Environment Variables (recommended)

Best practice: use environment variables to switch providers without code changes.

```bash
# .env
RPC_URL=https://shared.eu-central-1.getblock.io/YOUR_GETBLOCK_TOKEN/
```

```javascript
const provider = new JsonRpcProvider(process.env.RPC_URL);
```

Load the `.env` file when you run the script, either with `node --env-file=.env your-script.js` (Node.js 20.6+) or by calling `require('dotenv').config()` at the top of your code.
{% endstep %}

{% step %}
#### Verify Everything Works

Run a quick check:

{% code overflow="wrap" %}
```bash
# Load .env into the current shell so $RPC_URL is set
set -a && source .env && set +a

curl -X POST $RPC_URL \
  -H "Content-Type: application/json" \
  -d '{"jsonrpc":"2.0","method":"eth_blockNumber","params":[],"id":1}'
```
{% endcode %}

If you get a valid response with a block number, you're good.
{% endstep %}
{% endstepper %}

## Feature Mapping: Alchemy → GetBlock

<table data-search="false"><thead><tr><th>Alchemy Feature</th><th>GetBlock Equivalent</th><th>Notes</th></tr></thead><tbody><tr><td>RPC Endpoints</td><td>✅ Shared Nodes</td><td>Same JSON-RPC methods</td></tr><tr><td>WebSocket</td><td>✅ WebSocket support</td><td>Same subscription methods</td></tr><tr><td>Archive data</td><td>✅ Archive mode</td><td>Available on all plans (Alchemy requires paid)</td></tr><tr><td>Debug/Trace</td><td>✅ Trace &#x26; Debug</td><td>Available on Starter+</td></tr><tr><td>NFT API</td><td>❌ Not available</td><td>Use third-party NFT APIs</td></tr><tr><td>Token API</td><td>❌ Not available</td><td>Use standard RPC methods or third-party</td></tr><tr><td>Notify (webhooks)</td><td>✅ Blockchain Tracker</td><td>GetBlock's webhook solution</td></tr><tr><td>Enhanced APIs</td><td>Standard RPC</td><td>GetBlock focuses on RPC infrastructure</td></tr><tr><td>Alchemy SDK</td><td>Standard libraries</td><td>Use ethers.js, web3.js directly</td></tr><tr><td>Dashboard analytics</td><td>✅ Statistics</td><td>Method tracking, CU monitoring</td></tr><tr><td>Multi-chain</td><td>✅ 130+ chains</td><td>More chains than Alchemy (~70)</td></tr><tr><td>Regional selection</td><td>✅ 3 regions</td><td>Alchemy doesn't offer explicit region selection</td></tr><tr><td>Dedicated Nodes</td><td>✅ Dedicated Nodes</td><td>From $1,000/mo, unlimited requests</td></tr><tr><td>Team accounts</td><td>✅ Team accounts</td><td>Up to 30 users, role-based access</td></tr></tbody></table>

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

const getblock = new JsonRpcProvider("https://shared.eu-central-1.getblock.io/YOUR_TOKEN/");
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

<a href="https://account.getblock.io" class="button primary">Ready to switch? and have your new endpoints running in under 5 minutes.</a>
