---
description: >-
  Learn how to build an agent that answers natural-language questions about
  Robinhood Chain
---

# How To Build an AI Agent on Robinhood Chain with GetBlock RPC

Robinhood Chain is Robinhood's Arbitrum-based Ethereum L2, built for tokenized stocks, real-world assets, and AI agents — it produces blocks roughly every 100ms. For an AI agent to be useful on a chain like this, it needs live onchain data: balances, blocks, transactions, and token state.&#x20;

Large language models cannot read a blockchain on their own, and if you simply ask one about onchain values, it will guess confidently and wrongly. Raw JSON-RPC, on the other hand, is too low-level to hand to a model directly: hex quantities, wei amounts, and positional params invite unit mistakes. The missing piece is a thin tool layer that turns natural-language questions into real RPC calls and feeds human-readable results back to the model.

In this guide, you will learn how to build an AI agent that answers natural-language questions about Robinhood Chain using OpenAI function calling for reasoning and GetBlock RPC for live chain reads.

### What you'll build

An agentic loop in `agent.js` — driven by `npm run agent -- "<question>"` — that:

1. Sends your question to an OpenAI model along with five JSON-schema tool definitions.
2. Executes every tool call the model requests as a real JSON-RPC call against your GetBlock endpoint.
3. Feeds the formatted results (`balanceEth`, `gasPriceGwei`) back to the model as `tool` messages.
4. Repeats until the model has enough data, then prints its plain-text answer with Blockscout explorer links.

{% hint style="success" %}
#### Optional

This guide also contains two helper scripts along the way:

1. &#x20;`quickstart.js` to verify your endpoint,
2. &#x20;`watch-blocks.js` to stream the \~100ms block cadence over WebSocket.

You can skip this if you want too
{% endhint %}

### Prerequisites

* **Node.js 18+** — the project uses ES modules and top-level `await`.
* A [**GetBlock account**](https://account.getblock.io/).
* A **Robinhood Chain RPC endpoint** — created from your GetBlock dashboard (HTTP, plus WebSocket if you want block streaming).
* An [**OpenAI API key**](https://platform.openai.com/) needed only for the agent step.
* Basic JavaScript knowledge.

### Project Setup

{% stepper %}
{% step %}
#### Create the project and install dependencies

Scaffold a Node project and install `viem` (typed RPC client), `openai` (function calling), and `dotenv`:

```bash
mkdir robinhood-ai-agent && cd robinhood-ai-agent
npm init -y
npm install viem dotenv openai
```

Set `"type": "module"` and the run scripts in `package.json`:

{% code title="package.json" %}
```json
{
  "name": "robinhood-ai-agent",
  "type": "module",
  "scripts": {
    "quickstart": "node quickstart.js",
    "watch": "node watch-blocks.js",
    "agent": "node agent.js"
  }
}
```
{% endcode %}
{% endstep %}

{% step %}
#### Configure your endpoints and keys

In your [GetBlock dashboard](https://account.getblock.io/), click **Get an endpoint**, select **Robinhood Chain** (mainnet), and copy the **JSON-RPC (HTTP)** and **WebSocket** URLs. Store them in a `.env` file:

{% code title=".env" %}
```bash
GETBLOCK_RPC_URL=https://go.getblock.io/<YOUR_ACCESS_TOKEN>/
GETBLOCK_WS_URL=wss://go.getblock.io/<YOUR_ACCESS_TOKEN>/
OPENAI_API_KEY=sk-...
```
{% endcode %}

{% hint style="warning" %}
The access token in the URL path is how GetBlock authenticates you — no headers needed. Never commit `.env`; keep it in `.gitignore`.
{% endhint %}
{% endstep %}

{% step %}
#### Define the chain and clients

Robinhood Chain isn't bundled in viem yet, so define it with `defineChain` and point both transports at GetBlock:

{% code title="chain.js" overflow="wrap" %}
```js
import "dotenv/config";
import { createPublicClient, defineChain, http, webSocket } from "viem";

const RPC_URL = process.env.GETBLOCK_RPC_URL;
const WS_URL = process.env.GETBLOCK_WS_URL;

if (!RPC_URL) {
  throw new Error(
    "GETBLOCK_RPC_URL is not set. Copy .env.example to .env and paste your GetBlock Robinhood Chain endpoint."
  );
}

// Robinhood Chain mainnet — an Arbitrum-based Ethereum L2 with ~100ms blocks.
// Chain ID 4663 (testnet: 46630). Gas is paid in ETH.
export const robinhoodChain = defineChain({
  id: 4663,
  name: "Robinhood Chain",
  nativeCurrency: { name: "Ether", symbol: "ETH", decimals: 18 },
  rpcUrls: {
    default: {
      http: [RPC_URL],
      ...(WS_URL ? { webSocket: [WS_URL] } : {}),
    },
  },
  blockExplorers: {
    default: {
      name: "Blockscout",
      url: "https://robinhoodchain.blockscout.com",
    },
  },
});

export const publicClient = createPublicClient({
  chain: robinhoodChain,
  transport: http(RPC_URL),
});

export function createWsClient() {
  if (!WS_URL) {
    throw new Error("GETBLOCK_WS_URL is not set — WebSocket subscriptions need it.");
  }
  return createPublicClient({
    chain: robinhoodChain,
    transport: webSocket(WS_URL),
  });
}
```
{% endcode %}

Verify the connection with a small script:

{% code title="quickstart.js" overflow="wrap" %}
```js
import { formatGwei } from "viem";
import { publicClient } from "./chain.js";

const [chainId, blockNumber, gasPrice, block] = await Promise.all([
  publicClient.getChainId(),
  publicClient.getBlockNumber(),
  publicClient.getGasPrice(),
  publicClient.getBlock(),
]);

console.log("✅ Connected to Robinhood Chain via GetBlock\n");
console.log(`Chain ID:      ${chainId} ${chainId === 4663 ? "(mainnet)" : chainId === 46630 ? "(testnet)" : ""}`);
console.log(`Latest block:  #${blockNumber}`);
console.log(`Gas price:     ${formatGwei(gasPrice)} gwei`);
console.log(`Block time:    ${new Date(Number(block.timestamp) * 1000).toISOString()}`);
console.log(`Transactions:  ${block.transactions.length} in latest block`);
console.log(`Explorer:      https://robinhoodchain.blockscout.com/block/${blockNumber}`);
```
{% endcode %}

Run `npm run quickstart` — if the chain ID prints `4663`, as seen below, your endpoint is live.

{% code title="terminal" overflow="wrap" %}
```bash

✅ Connected to Robinhood Chain via GetBlock

Chain ID:      4663 (mainnet)
Latest block:  #24050108
Gas price:     0.020092 gwei
Block time:    2026-07-31T08:55:55.000Z
Transactions:  4 in latest block
Explorer:      https://robinhoodchain.blockscout.com/block/24050108
```
{% endcode %}
{% endstep %}

{% step %}
#### Stream blocks over WebSocket (optional)

Robinhood Chain targets \~100ms blocks. GetBlock's WebSocket endpoint exposes `eth_subscribe`, which viem wraps in `watchBlocks`:

{% code title="watch-blocks.js" overflow="wrap" %}
```js
import { createWsClient } from "./chain.js";

const wsClient = createWsClient();
let lastTimestamp = null;

console.log("👀 Watching Robinhood Chain blocks (Ctrl+C to stop)...\n");

const unwatch = wsClient.watchBlocks({
  onBlock(block) {
    const delta =
      lastTimestamp !== null ? `+${Number(block.timestamp - lastTimestamp)}s` : "—";
    lastTimestamp = block.timestamp;
    // newHeads subscriptions deliver headers only — tx list may be absent
    const txs = block.transactions ? String(block.transactions.length).padStart(3) : "  ?";
    console.log(`#${block.number}  txs: ${txs}  gasUsed: ${block.gasUsed}  Δ ${delta}`);
  },
  onError(error) {
    console.error("Watch error:", error.message);
  },
});

process.on("SIGINT", () => {
  unwatch();
  process.exit(0);
});
```
{% endcode %}

Run `npm run watch` and note the `Δ +0s` deltas — multiple blocks land within the same second.&#x20;

{% code title="terminal" overflow="wrap" %}
```bash
#24051177  txs:   4  gasUsed: 354774  Δ —
#24051178  txs:   8  gasUsed: 656380  Δ +0s
#24051179  txs:   5  gasUsed: 7442026  Δ +0s
#24051180  txs:   7  gasUsed: 730819  Δ +1s
#24051181  txs:   6  gasUsed: 714174  Δ +0s
```
{% endcode %}

That near-instant confirmation feedback is exactly why the chain suits agent-driven trading.
{% endstep %}

{% step %}
#### Build the agent: tools, implementations, and the loop

This is the core of the tutorial. The pattern is **function calling**: you describe tools to the model with JSON schemas, it decides when to call them, your code executes the real RPC calls, and the model reasons over the results.

The agent gets five read-only tools, all backed by your GetBlock endpoint:

| Tool               | RPC methods behind it                                                    | The agent uses it when…           |
| ------------------ | ------------------------------------------------------------------------ | --------------------------------- |
| `get_chain_status` | `eth_chainId`, `eth_blockNumber`, `eth_gasPrice`, `eth_getBlockByNumber` | asked about the network or gas    |
| `get_balance`      | `eth_getBalance`                                                         | asked what a wallet holds         |
| `get_block`        | `eth_getBlockByNumber`                                                   | asked about recent activity       |
| `get_transaction`  | `eth_getTransactionByHash`, `eth_getTransactionReceipt`                  | given a tx hash                   |
| `get_token_info`   | `eth_call` (ERC-20 reads)                                                | asked about a token / Stock Token |

{% code title="agent.js" overflow="wrap" %}
```js
import "dotenv/config";
import OpenAI from "openai";
import { formatEther, formatGwei, formatUnits, parseAbi } from "viem";
import { publicClient } from "./chain.js";

const client = new OpenAI();
const MODEL = process.env.OPENAI_MODEL ?? "gpt-5.1";

const erc20Abi = parseAbi([
  "function name() view returns (string)",
  "function symbol() view returns (string)",
  "function decimals() view returns (uint8)",
  "function totalSupply() view returns (uint256)",
  "function balanceOf(address) view returns (uint256)",
]);

// ---------- Tool schemas the model sees ----------

const tools = [
  {
    type: "function",
    function: {
      name: "get_chain_status",
      description:
        "Get the current state of Robinhood Chain: chain ID, latest block number, gas price, and base fee. " +
        "Call this when the user asks about the network, gas costs, or how the chain is doing right now.",
      parameters: { type: "object", properties: {}, additionalProperties: false },
    },
  },
  {
    type: "function",
    function: {
      name: "get_balance",
      description:
        "Get the native ETH balance of an address on Robinhood Chain. " +
        "Call this when the user asks how much ETH a wallet or contract holds.",
      parameters: {
        type: "object",
        properties: {
          address: { type: "string", description: "0x-prefixed EVM address" },
        },
        required: ["address"],
        additionalProperties: false,
      },
    },
  },
  {
    type: "function",
    function: {
      name: "get_block",
      description:
        "Fetch a block by number (or the latest block if no number is given): timestamp, transaction count, " +
        "gas used, and transaction hashes. Call this when the user asks what happened in a block or about recent activity.",
      parameters: {
        type: "object",
        properties: {
          blockNumber: {
            type: "string",
            description: "Block number as a decimal string. Omit for the latest block.",
          },
        },
        additionalProperties: false,
      },
    },
  },
  {
    type: "function",
    function: {
      name: "get_transaction",
      description:
        "Look up a transaction by hash: sender, recipient, value, gas, and confirmation status. " +
        "Call this when the user provides a transaction hash.",
      parameters: {
        type: "object",
        properties: {
          hash: { type: "string", description: "0x-prefixed transaction hash" },
        },
        required: ["hash"],
        additionalProperties: false,
      },
    },
  },
  {
    type: "function",
    function: {
      name: "get_token_info",
      description:
        "Read an ERC-20 token contract on Robinhood Chain (including tokenized Stock Tokens): name, symbol, " +
        "decimals, total supply, and optionally a holder's balance. Call this when the user asks about a token " +
        "or a tokenized stock at a given contract address.",
      parameters: {
        type: "object",
        properties: {
          tokenAddress: { type: "string", description: "0x-prefixed token contract address" },
          holderAddress: {
            type: "string",
            description: "Optional 0x-prefixed address to check the token balance of",
          },
        },
        required: ["tokenAddress"],
        additionalProperties: false,
      },
    },
  },
];

// ---------- Tool implementations backed by GetBlock RPC ----------

const implementations = {
  async get_chain_status() {
    const [chainId, blockNumber, gasPrice, block] = await Promise.all([
      publicClient.getChainId(),
      publicClient.getBlockNumber(),
      publicClient.getGasPrice(),
      publicClient.getBlock(),
    ]);
    return {
      chainId,
      network: chainId === 4663 ? "mainnet" : chainId === 46630 ? "testnet" : "unknown",
      latestBlock: blockNumber.toString(),
      gasPriceGwei: formatGwei(gasPrice),
      baseFeeGwei: block.baseFeePerGas ? formatGwei(block.baseFeePerGas) : null,
      latestBlockTimestamp: new Date(Number(block.timestamp) * 1000).toISOString(),
      txCountInLatestBlock: block.transactions.length,
    };
  },

  async get_balance({ address }) {
    const balance = await publicClient.getBalance({ address });
    return {
      address,
      balanceEth: formatEther(balance),
      balanceWei: balance.toString(),
      explorer: `https://robinhoodchain.blockscout.com/address/${address}`,
    };
  },

  async get_block({ blockNumber }) {
    const block = await publicClient.getBlock(
      blockNumber ? { blockNumber: BigInt(blockNumber) } : {}
    );
    return {
      number: block.number.toString(),
      timestamp: new Date(Number(block.timestamp) * 1000).toISOString(),
      txCount: block.transactions.length,
      gasUsed: block.gasUsed.toString(),
      baseFeeGwei: block.baseFeePerGas ? formatGwei(block.baseFeePerGas) : null,
      transactions: block.transactions.slice(0, 10),
      explorer: `https://robinhoodchain.blockscout.com/block/${block.number}`,
    };
  },

  async get_transaction({ hash }) {
    const [tx, receipt] = await Promise.all([
      publicClient.getTransaction({ hash }),
      publicClient.getTransactionReceipt({ hash }).catch(() => null),
    ]);
    return {
      hash,
      from: tx.from,
      to: tx.to,
      valueEth: formatEther(tx.value),
      blockNumber: tx.blockNumber?.toString() ?? "pending",
      status: receipt ? receipt.status : "pending",
      gasUsed: receipt?.gasUsed?.toString() ?? null,
      explorer: `https://robinhoodchain.blockscout.com/tx/${hash}`,
    };
  },

  async get_token_info({ tokenAddress, holderAddress }) {
    const contract = { address: tokenAddress, abi: erc20Abi };
    const [name, symbol, decimals, totalSupply] = await Promise.all([
      publicClient.readContract({ ...contract, functionName: "name" }),
      publicClient.readContract({ ...contract, functionName: "symbol" }),
      publicClient.readContract({ ...contract, functionName: "decimals" }),
      publicClient.readContract({ ...contract, functionName: "totalSupply" }),
    ]);
    const result = {
      tokenAddress,
      name,
      symbol,
      decimals,
      totalSupply: formatUnits(totalSupply, decimals),
      explorer: `https://robinhoodchain.blockscout.com/token/${tokenAddress}`,
    };
    if (holderAddress) {
      const balance = await publicClient.readContract({
        ...contract,
        functionName: "balanceOf",
        args: [holderAddress],
      });
      result.holderAddress = holderAddress;
      result.holderBalance = formatUnits(balance, decimals);
    }
    return result;
  },
};

// ---------- The agentic loop ----------

const SYSTEM_PROMPT =
  "You are an onchain analyst agent for Robinhood Chain — Robinhood's Arbitrum-based Ethereum L2 for " +
  "tokenized stocks, real-world assets, and AI agents. You have read-only JSON-RPC access via GetBlock. " +
  "Use your tools to fetch live data before answering; never guess onchain values. Amounts from tools are " +
  "already human-readable (ETH, gwei, token units). Keep answers concise and include Blockscout explorer " +
  "links when they help the user verify.";

const question =
  process.argv.slice(2).join(" ") ||
  "Give me a status report on Robinhood Chain: latest block, gas price, and recent activity.";

console.log(`🤖 Question: ${question}\n`);

const messages = [
  { role: "system", content: SYSTEM_PROMPT },
  { role: "user", content: question },
];

const MAX_TURNS = 10;

for (let turn = 0; turn < MAX_TURNS; turn++) {
  const response = await client.chat.completions.create({
    model: MODEL,
    messages,
    tools,
  });

  const message = response.choices[0].message;
  messages.push(message);

  // No more tool calls — the model has its final answer
  if (!message.tool_calls?.length) {
    console.log(message.content);
    break;
  }

  // Execute every tool call and feed the results back
  for (const call of message.tool_calls) {
    const { name, arguments: rawArgs } = call.function;
    console.log(`  ⚙️  ${name}(${rawArgs})`);
    let result;
    try {
      result = await implementations[name](JSON.parse(rawArgs || "{}"));
    } catch (error) {
      result = { error: error.shortMessage ?? error.message };
    }
    messages.push({
      role: "tool",
      tool_call_id: call.id,
      content: JSON.stringify(result),
    });
  }
}
```
{% endcode %}

Two details worth copying into your own agents:

* **Prescriptive tool descriptions.** Each description says _when_ to call the tool, not just what it does — this measurably improves the model's tool selection.
* **Human-readable tool results.** Tools return formatted values (`balanceEth`, `gasPriceGwei`) rather than raw wei, so the model makes fewer unit mistakes and the answers read better.
{% endstep %}

{% step %}
#### Run the agent

Ask it anything about the chain:

```bash
npm run agent -- "What's the current gas price and latest block on Robinhood Chain?"
```

Expected output:

{% code title="terminal" %}
```bash
🤖 Question: What's the current gas price and latest block on Robinhood Chain?

  ⚙️  get_chain_status({})
Robinhood Chain (mainnet, chain ID 4663) is at block #18,452,940. Gas is
currently 0.01 gwei — effectively free for reads and cheap for agent-driven
trades. The latest block landed at 14:32:07 UTC with 3 transactions.
```
{% endcode %}

The `⚙️` lines show each RPC-backed tool call as the model makes it. Try chained questions — _"Analyze wallet 0x…"_ typically triggers `get_balance` then `get_chain_status` before the model writes its answer.
{% endstep %}
{% endstepper %}

## Troubleshooting

<table data-search="false"><thead><tr><th>Problem</th><th>Likely cause</th><th>Fix</th></tr></thead><tbody><tr><td><code>GETBLOCK_RPC_URL is not set</code> on startup</td><td><code>.env</code> missing or not copied from the example</td><td>Copy <code>.env.example</code> to <code>.env</code> and paste your dashboard URLs.</td></tr><tr><td>Chain ID prints something other than <code>4663</code></td><td>The access token is for a different chain</td><td>Create a <strong>Robinhood Chain</strong> endpoint in the dashboard and use that token.</td></tr><tr><td><code>HTTP 401</code> / <code>403</code> from <code>go.getblock.io</code></td><td>Invalid, revoked, or mistyped access token</td><td>Re-copy the exact URL (including trailing <code>/</code>) from the dashboard.</td></tr><tr><td><code>GETBLOCK_WS_URL is not set</code> when running <code>npm run watch</code></td><td>Only the HTTP endpoint was configured</td><td>Add the WebSocket URL from the same endpoint page.</td></tr><tr><td><code>watchBlocks</code> prints <code>txs: ?</code></td><td><code>newHeads</code> delivers headers only on some nodes</td><td>Expected — fetch the full block with <code>get_block</code> when you need the tx list.</td></tr><tr><td>OpenAI <code>401</code> or model errors in <code>agent.js</code></td><td>Missing/invalid <code>OPENAI_API_KEY</code> or unavailable model</td><td>Set the key; override the model with <code>OPENAI_MODEL</code> if needed.</td></tr><tr><td>Agent answers without calling tools</td><td>Vague tool descriptions after edits</td><td>Keep the "Call this when…" phrasing in every tool description.</td></tr></tbody></table>

### Conclusion

You built an AI agent that reasons over live Robinhood Chain data: a Viemviem client pointed at a GetBlock RPC endpoint, five read-only JSON-RPC tools described to an OpenAI model as function-calling schemas, and an agentic loop that executes the model's tool calls and feeds formatted results back until it can answer in plain text. The same pattern — prescriptive tool descriptions, human-readable tool outputs, and a bounded tool loop — extends directly to write operations, event monitoring, and multi-turn onchain copilots.

### Resources

* [GetBlock dashboard — get a Robinhood Chain endpoint](https://account.getblock.io/)
* [Robinhood Chain explorer (Blockscout)](https://robinhoodchain.blockscout.com/)
* [OpenAI function calling guide](https://platform.openai.com/docs/guides/function-calling)
* [viem documentation](https://viem.sh/)
* [Project repo](https://github.com/GetBlock-io/guides/tree/main/robinhood-ai-agent)
