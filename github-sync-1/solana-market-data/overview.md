---
description: Access low-latency Solana market data through HTTP, WebSocket, and gRPC.
---

# Overview

Solana Market Data gives you **low-latency, ready-to-use market data from Solana** without the need to build and maintain your own indexing and data-processing infrastructure.

Instead of working with raw blockchain transactions, decoding different DEX[^1] programs, and building aggregation pipelines yourself, you can consume already processed market data through a simple API.

{% hint style="info" %}
**Solana Market Data** is designed for applications that need **live, up-to-date trading data** — from trading dashboards and analytics platforms to automated and agentic trading systems.
{% endhint %}

### What you can get

**Solana Market Data** provides trading information aggregated across supported Solana venues.

Depending on how much processing you want to handle yourself, you can work with:

<table data-search="false"><thead><tr><th>Topic</th><th>Description</th><th>Required parameters</th></tr></thead><tbody><tr><td><code>trades</code></td><td>Normalized individual trades</td><td>None</td></tr><tr><td><code>block</code></td><td>Trading activity aggregated for one Solana slot</td><td>None</td></tr><tr><td><code>ohlcv</code></td><td>Open, high, low, close, and volume candles</td><td><code>window</code></td></tr><tr><td><code>twap</code></td><td>Time-weighted average price</td><td><code>window</code></td></tr><tr><td><code>vwap</code></td><td>Volume-weighted average price</td><td><code>window</code></td></tr><tr><td><code>volume</code></td><td>Trading volume and swap activity</td><td><code>window</code></td></tr><tr><td><code>token</code></td><td>Token metadata</td><td>None</td></tr></tbody></table>

You can choose between lower-level trade data and higher-level market metrics that are already calculated for you.

### Why use Solana Market Data?

Building the same pipeline yourself requires you to collect Solana transactions, support multiple trading venues, decode swaps, normalize the data, and continuously calculate market metrics.

Solana Market Data handles this processing for you.

This allows you to focus on building your:

* trading application;
* analytics platform;
* market dashboard;
* trading strategy;
* AI or automated trading agent.

### How to use it

Getting started is straightforward:

1. **Activate Solana Market Data** and get access with your API key.
2. **Choose the data you need** — for example Trades, VWAP, Volume, or Candles.
3. **Query or stream the data** using HTTP, WebSocket, or Yellowstone-compatible gRPC.

{% hint style="info" %}
You can also use the [**Playground**](https://account.getblock.io/products/solana-data-stream#playground) to explore available methods and see the data before integrating it into your application.
{% endhint %}

### When to use the topics?

<table data-search="false"><thead><tr><th>If you need</th><th>Use</th></tr></thead><tbody><tr><td>Individual trades and maximum calculation flexibility</td><td><code>trades</code></td></tr><tr><td>Market activity for each Solana slot</td><td><code>block</code></td></tr><tr><td>Candles or price charts</td><td><code>ohlcv</code></td></tr><tr><td>A price weighted by elapsed time</td><td><code>twap</code></td></tr><tr><td>A price weighted by traded volume</td><td><code>vwap</code></td></tr><tr><td>Buy, sell, and total market activity</td><td><code>volume</code></td></tr><tr><td>Token metadata</td><td><code>token</code></td></tr></tbody></table>

### Next steps

Explore the available **Market Data methods**, learn how to **connect to the API**, or try the service directly in the [**Playground**](https://account.getblock.io/products/solana-data-stream#playground).

[^1]: Decentralized Exchange
