# Overview

**GetBlock EVM Stream delivers blockchain events in real time for wallets, payment services, DeFi applications, and analytics platforms.** You select a network, event types, and filters, then receive updates over a persistent WebSocket connection.

This reduces the need to continuously poll RPC endpoints, extract events from blocks, and maintain event delivery infrastructure. Filters for addresses, contracts, and other parameters let applications receive the data they need while saving cost.

`stream-service` handles data collection and processing, while `stream-api` provides customer access. Together, they form a single product with a consistent integration approach across supported EVM networks.

{% hint style="warning" %}
This version currently focuses on live events; historical exports and backfilling missed events are outside its scope.
{% endhint %}

### Use cases and supported topics

With EVM Data stream, you can carry out the following operations:&#x20;

<table data-search="false"><thead><tr><th>Business use case</th><th>What customers receive</th><th>Topics</th></tr></thead><tbody><tr><td><strong>Payments and wallets</strong></td><td>ERC-20 transfers, transaction inclusion in a block, and execution results for incoming payment notifications and status updates</td><td><code>erc20Transfers</code>, <code>newMinedTransactions</code>, <code>transactionReceipts</code></td></tr><tr><td><strong>Token approval monitoring</strong></td><td>Events granting or changing ERC-20 spending allowances for alerts and risk monitoring</td><td><code>erc20Allowence</code></td></tr><tr><td><strong>DeFi and smart contract events</strong></td><td>Contract logs, including events decoded using the customer's ABI, such as swaps, deposits, and loans</td><td><code>logs</code>, <code>decodedLogs</code></td></tr><tr><td><strong>NFT applications and marketplaces</strong></td><td>NFT transfers, mints/burns, approvals, and metadata update signals</td><td><code>nftTransfers</code>, <code>nftApprovals</code>, <code>nftMetadataUpdates</code></td></tr><tr><td><strong>Smart wallets / Account Abstraction</strong></td><td>Smart account operation events through EntryPoint, including execution and errors</td><td><code>accountAbstractionOperations</code></td></tr><tr><td><strong>Pending transaction monitoring</strong></td><td>Transactions observed in the mempool before inclusion in a block</td><td><code>newPendingTransactions</code></td></tr><tr><td><strong>Network analytics and custom indexing</strong></td><td>Block headers or full blocks to update applications and process new data</td><td><code>newHeads</code>, <code>newBlocks</code></td></tr><tr><td><strong>Execution and internal activity analysis</strong></td><td>Call traces at the block or individual transaction level for analytics and troubleshooting</td><td><code>newBlockTraces</code>, <code>newMinedTransactionsTraces</code></td></tr></tbody></table>

{% hint style="info" %}
Mempool and trace availability depends on each network's capabilities and configuration. Inclusion in a block does not itself imply transaction finality; when reorganization handling is enabled, Stream also delivers correction events.
{% endhint %}

### Supported Networks

Currently, you can make use of stream data on four networks, which are:

1. BNB Smart Mainnet Chain
2. Robinhood Mainnet Chain
3. Polygon Mainnet
4. Ethereum Mainnet Chain

{% hint style="info" %}
While Optimism and Base chains are in the pipeline, we will add more EVM-compatible chains over time.&#x20;
{% endhint %}

### Pricing

EVM data stream charged by usage using Compute unit (CU)[^1]. Standard pricing costs 10 CU per delivered event. Usage depends on the number of notifications delivered to the customer after filters are applied; for example;&#x20;

* 1,000 events = **10,000 CU**.
* 100,000 events = **1,000,000 CU**.
* 1,000,000 events = **10,000,000 CU**.

Each notification counts separately. One transaction can generate multiple notifications; delivering the same event to multiple subscriptions is counted separately. Precise filters help customers control data volume and CU consumption.

{% hint style="info" %}
Successful subscription creation abd cancellation cost 10 CU each
{% endhint %}

[^1]: **Compute Unit (CU)** is a weighted metric used by GetBlock to measure the computational effort required to fulfill a specific JSON-RPC API reques
