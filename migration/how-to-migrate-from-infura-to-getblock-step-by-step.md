---
description: >-
  Switch from Infura to GetBlock in minutes. Migration guide covering endpoint
  URLs, SDK changes, feature mapping,  and why teams are making the switch.
---

# How to Migrate from Infura to GetBlock — Step-by-Step

Infura was the original Ethereum RPC provider, but by 2026, the landscape had changed. If you're looking for more chain support, better pricing, or geographic endpoint control, migrating from Infura to GetBlock is straightforward. This migration only takes 5–10 minutes.

## Why Teams Migrate from Infura

| Reason                 | Infura                          | GetBlock                                         |
| ---------------------- | ------------------------------- | ------------------------------------------------ |
| Chain coverage         | \~15 networks                   | **100+ blockchains**                             |
| Free tier archive data | Paid plans only                 | **All plans including Free**                     |
| Region selection       | No explicit choice              | **Frankfurt, New York, Singapore**               |
| Dedicated nodes        | Custom pricing                  | **From $1,000/mo, transparent**                  |
| Solana support         | Limited                         | **Full: RPC + gRPC + StreamFirst + LandFirst**   |
| Non-EVM chains         | Minimal (ETH-focused)           | **Bitcoin, Litecoin, Dogecoin, TRON, TON, etc.** |
| Pricing clarity        | Credit-based (varies by method) | **CU-based with clear tiers**                    |

Infura is excellent for Ethereum-first projects, but if your stack is growing beyond Ethereum, GetBlock's 100+ chain coverage eliminates the need for multiple providers.

### How to Switch From Infura To GetBlock

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
https://mainnet.infura.io/v3/YOUR_PROJECT_ID
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
  "https://mainnet.infura.io/v3/YOUR_PROJECT_ID"
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
  "https://mainnet.infura.io/v3/YOUR_PROJECT_ID"
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
w3 = Web3(Web3.HTTPProvider("https://mainnet.infura.io/v3/YOUR_PROJECT_ID"))

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
    url: "https://mainnet.infura.io/v3/YOUR_PROJECT_ID"
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
mainnet = "https://mainnet.infura.io/v3/YOUR_PROJECT_ID"

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
const wsProvider = new WebSocketProvider("wss://mainnet.infura.io/v3/YOUR_PROJECT_ID");

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

### Multi-Chain Migration

If you're using Infura for multiple chains, you'll need a separate GetBlock endpoint for each chain. Create them all in one dashboard session:

```bash
# .env — Before (Infura)
ETH_RPC=https://mainnet.infura.io/v3/PROJECT_ID
POLYGON_RPC=https://polygon-mainnet.infura.io/v3/PROJECT_ID

# .env — After (GetBlock)
ETH_RPC=https://go.getblock.io/ETH_TOKEN/
POLYGON_RPC=https://go.getblock.io/POLYGON_TOKEN/
# Plus 90+ more chains you can now access:
SOLANA_RPC=https://go.getblock.io/SOL_TOKEN/
BSC_RPC=https://go.getblock.io/BSC_TOKEN/
TON_RPC=https://go.getblock.io/TON_TOKEN/
```

## Feature Mapping: Infura → GetBlock

| Infura Feature      | GetBlock Equivalent                        |
| ------------------- | ------------------------------------------ |
| JSON-RPC            | ✅ Full JSON-RPC support                    |
| WebSocket           | ✅ WebSocket support                        |
| Archive data        | ✅ All plans (Infura: paid only)            |
| IPFS                | ❌ Not available (use Pinata, nft.storage)  |
| Gas API             | ✅ `eth_gasPrice`, `eth_feeHistory` methods |
| Ethereum Beacon API | ✅ Available                                |
| MetaMask default    | Can be configured as custom RPC            |
| \~15 chains         | ✅ **100+ chains**                          |
| Dedicated Nodes     | ✅ From $1,000/mo                           |

### What You Gain

* **85+ additional blockchains** — Bitcoin, Solana, BSC, TON, TRON, and more
* **Archive data at no extra cost** — even on the free plan
* **Regional endpoints** — control where your requests are processed
* **Dedicated Nodes with clear pricing** — unlimited requests from $1,000/mo
* **Advanced Solana and BSC tooling** — HFT infrastructure, private mempool

### What You'll Need Alternatives For

* **Infura IPFS** → Pinata, nft.storage, or Fleek
* **MetaMask default integration** → Add GetBlock as custom RPC in MetaMask settings

_Ready to switch?_ <a href="https://account.getblock.io" class="button primary"> Create your free GetBlock account</a>_and have your new endpoints running in under 5 minutes._
