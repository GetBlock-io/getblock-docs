---
description: >-
  Query curated blockchain market data from Solana, Polymarket, Hyperliquid, and
  Robinhood Chain with standard SQL.
---

# Overview

Market SQL gives you **curated, query-ready blockchain market data** you can explore with SQL, without stitching together RPC calls, parsing raw transactions, or building your own data pipelines.

Instead of learning which endpoint to call and reconstructing the information you need from raw blockchain data, you choose a dataset, connect a SQL-compatible client, and query the tables directly over the ClickHouse[^1] HTTPS protocol.

{% hint style="warning" %}
**Market SQL** is designed for **research, backtesting, analytics, and the development of data products**. It is read-only and is not a live trading signal feed or WebSocket service. For live Solana market data, use [**Solana Market Data**](../solana-market-data/overview.md).
{% endhint %}

### What you can get

**Market SQL** provides 53 curated tables across four datasets. Each dataset is subscribed to separately.

<table data-search="false"><thead><tr><th>Dataset</th><th>Description</th><th>Database name</th></tr></thead><tbody><tr><td><a href="api-reference/solana.md">Solana</a></td><td>Swaps and launches across Pump.fun, PumpSwap, Raydium, and Meteora, plus token transfers, SOL funding activity, blocks, timestamps, and Jito tips</td><td><code>solana</code></td></tr><tr><td><a href="api-reference/hyperliquid.md">Hyperliquid</a></td><td>Order fills, perpetual-market activity, wallet positions, and aggregated order analytics</td><td><code>hyperliquid</code></td></tr><tr><td><a href="api-reference/robinhood.md">Robinhood Chain</a></td><td>Token launches, bonding-curve trading, migrations, Uniswap v3 and v4 pools and trades, and token metadata</td><td><code>robinhood</code></td></tr><tr><td><a href="api-reference/polymarket.md">Polymarket</a></td><td>Prediction-market order fills, event metadata, and market metadata</td><td><code>polymarket</code></td></tr></tbody></table>

Historical coverage varies by dataset.

### Why use Market SQL?

Traditional blockchain data access means knowing which method to call, which parameters to pass, and how to rebuild the answer from raw records. An API answers the questions it was designed to answer. SQL lets you ask your own.

With Market SQL, you do not need to understand RPC methods, transaction encoding, or indexing architecture before you start investigating a market. You start with a question, for example:

* Which tokens traded most actively?
* How did swap activity change over time?
* What happened around a particular launch?
* Which wallets or pools showed a specific pattern?

A single query can filter, aggregate, and join data server-side, so you do not have to download large volumes of raw records and process them locally.

This allows you to focus on:

* backtesting strategies with real trades;
* finding signals worth testing;
* building dashboards, research tools, and analytics systems;
* training models and powering AI agents with market data.

### How to use it

Getting started takes three steps:

1. **Choose a dataset and access period** under **Products → Market SQL** in your GetBlock account.
2. **Connect your SQL client** using your GetBlock user ID and API key over ClickHouse HTTPS.
3. **Query and iterate** using read-only SQL in the tools you already know.

{% hint style="info" %}
You can browse every dataset and table in the [**Datasets**](https://account.getblock.io/products/sql) tab of your dashboard before writing your first query.
{% endhint %}

### When to use which dataset?

<table data-search="false"><thead><tr><th>If you need</th><th>Use</th></tr></thead><tbody><tr><td>Solana DEX swaps, token launches, and bonding-curve migrations</td><td><code>solana</code></td></tr><tr><td>Wallet funding paths, token transfers, and Jito tips on Solana</td><td><code>solana</code></td></tr><tr><td>Perpetual fills, wallet PnL, and open positions</td><td><code>hyperliquid</code></td></tr><tr><td>Robinhood Chain launchpad activity and Uniswap v3/v4 trades</td><td><code>robinhood</code></td></tr><tr><td>Prediction-market fills, events, and outcomes</td><td><code>polymarket</code></td></tr></tbody></table>

### Pricing

Pricing is per dataset, and query limits depend on the selected access plan.

<table data-search="false"><thead><tr><th>Plan</th><th>Price</th><th>Best for</th></tr></thead><tbody><tr><td>Free trial</td><td>$0 for 2 hours</td><td>Exploring one dataset and running your first queries</td></tr><tr><td>24-hour trial</td><td>$29 for 24 hours</td><td>Validating a complete workflow or a specific investigation</td></tr><tr><td>Monthly</td><td>$299 per dataset per month</td><td>Ongoing research, analytics, bots, and model development</td></tr><tr><td>Custom</td><td>Contact sales</td><td>Custom query capacity and execution limits</td></tr></tbody></table>

{% hint style="info" %}
One free trial is available per account per dataset.
{% endhint %}

### Next steps

Learn how to **activate Market SQL and run your first query** in [**Getting Started**](getting-started.md), understand how queries work in [**Querying Market SQL**](querying-market-sql.md), or explore every table in the [**API Reference**](api-reference/).

[^1]: ClickHouse is an open-source, column-oriented database built for fast analytical queries over large datasets.
