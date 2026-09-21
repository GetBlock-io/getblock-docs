---
description: >-
  GetBlock EVM Data Stream delivers real-time blockchain events such as blocks,
  logs, token and NFT transfers, receipts, and traces over WebSocket on
  Ethereum, BNB Smart Chain, Polygon, and Robinhood.
---

# Overview

**GetBlock EVM Stream delivers blockchain events in real time for wallets, payment services, DeFi applications, and analytics platforms.** You select a network, event types, and filters, then receive updates over a persistent WebSocket connection.

This reduces the need to continuously poll RPC endpoints, extract events from blocks, and maintain event delivery infrastructure. Filters for addresses, contracts, and other parameters let applications receive the data they need while saving cost.

{% hint style="warning" %}
This version currently focuses on live events; historical exports and backfilling missed events are outside its scope.
{% endhint %}

### Use cases and supported topics

With EVM Data Stream, you can carry out the following operations:

<table data-search="false"><thead><tr><th>Business use case</th><th>What customers receive</th><th>Topics</th></tr></thead><tbody><tr><td><strong>Payments and wallets</strong></td><td>ERC-20 transfers, transaction inclusion in a block, and execution results for incoming payment notifications and status updates</td><td><code>erc20Transfers</code>, <code>newMinedTransactions</code>, <code>transactionReceipts</code></td></tr><tr><td><strong>Token approval monitoring</strong></td><td>Events granting or changing ERC-20 spending allowances for alerts and risk monitoring</td><td><code>erc20Allowence</code></td></tr><tr><td><strong>DeFi and smart contract events</strong></td><td>Contract logs, including events decoded using the customer's ABI, such as swaps, deposits, and loans</td><td><code>logs</code>, <code>decodedLogs</code></td></tr><tr><td><strong>NFT applications and marketplaces</strong></td><td>NFT transfers, mints/burns, approvals, and metadata update signals</td><td><code>nftTransfers</code>, <code>nftApprovals</code>, <code>nftMetadataUpdates</code></td></tr><tr><td><strong>Smart wallets / Account Abstraction</strong></td><td>Smart account operation events through EntryPoint, including execution and errors</td><td><code>accountAbstractionOperations</code></td></tr><tr><td><strong>Pending transaction monitoring</strong></td><td>Transactions observed in the mempool before inclusion in a block</td><td><code>newPendingTransactions</code></td></tr><tr><td><strong>Network analytics and custom indexing</strong></td><td>Block headers or full blocks to update applications and process new data</td><td><code>newHeads</code>, <code>newBlocks</code></td></tr><tr><td><strong>Execution and internal activity analysis</strong></td><td>Call traces at the block or individual transaction level for analytics and troubleshooting</td><td><code>newBlockTraces</code>, <code>newMinedTransactionsTraces</code></td></tr></tbody></table>

{% hint style="info" %}
Mempool and trace availability depends on each network's capabilities and configuration. Inclusion in a block does not itself imply transaction finality; when reorganization handling is enabled, Stream also delivers correction events.
{% endhint %}

### Supported Networks

EVM Data Stream is live on four networks. Each has its own WebSocket endpoint, and every topic is available on every network:

| Network           | WebSocket endpoint                                                  |
| ----------------- | ------------------------------------------------------------------- |
| Ethereum Mainnet  | `wss://stream.eu-central-1.getblock.io/v1/eth-mainnet/stream`       |
| BNB Smart Chain   | `wss://stream.eu-central-1.getblock.io/v1/bsc-mainnet/stream`       |
| Polygon Mainnet   | `wss://stream.eu-central-1.getblock.io/v1/polygon-mainnet/stream`   |
| Robinhood Mainnet | `wss://stream.eu-central-1.getblock.io/v1/robinhood-mainnet/stream` |

{% hint style="info" %}
Base and Avalanche are on the roadmap, and we will add more EVM-compatible chains over time.
{% endhint %}

### Pricing

EVM Data Stream is charged by usage in Compute Units (CU)[^1]. Standard pricing costs 10 CU per delivered event. Usage depends on the number of notifications delivered to the customer after filters are applied; for example;&#x20;

* 1,000 events = **10,000 CU**.
* 100,000 events = **1,000,000 CU**.
* 1,000,000 events = **10,000,000 CU**.

Each notification counts separately. One transaction can generate multiple notifications; delivering the same event to multiple subscriptions is counted separately. Precise filters help customers control data volume and CU consumption.

{% hint style="info" %}
Successful subscription creation and cancellation cost 10 CU each
{% endhint %}

### Next steps

* [Getting Started](getting-started.md): connect and receive your first events.
* [Handling Chain Reorganizations](handling-chain-reorganizations.md): keep your state correct when blocks are replaced.
* [API Reference](api-reference/): methods, all 15 topics, limits, and errors.

[^1]: **Compute Unit (CU)** is a weighted metric used by GetBlock to measure the computational effort required to fulfill a specific JSON-RPC API request.
