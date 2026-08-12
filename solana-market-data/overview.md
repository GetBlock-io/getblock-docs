# Overview

Solana Market Data gives you **low-latency, ready-to-use market data from Solana** without the need to build and maintain your own indexing and data-processing infrastructure.

Instead of working with raw blockchain transactions, decoding different DEX programs, and building aggregation pipelines yourself, you can consume already processed market data through a simple API.

{% hint style="info" %}
**Solana Market Data** is designed for applications that need **live and recent trading data** — from trading dashboards and analytics platforms to automated and agentic trading systems.
{% endhint %}

### What you can get

**Solana Market Data** provides trading information aggregated across supported Solana venues.

Depending on how much processing you want to handle yourself, you can work with:

* **Trades** — normalized individual trades for custom calculations and analytics.
* **Blocks** — trading activity aggregated for each Solana slot.
* **TWAP & VWAP** — ready-to-use average price metrics.
* **Volume** — buy, sell, and total trading activity over time.
* **Candles (OHLCV)** — ready-made price candles for charts and market analysis.
* **Buy/Sell Activity** — data for tracking the balance between buying and selling activity.

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

You can also use the **Playground** to explore available methods and see the data before integrating it into your application.

### Next steps

Explore the available **Market Data methods**, learn how to **connect to the API**, or try the service directly in the **Playground**.
