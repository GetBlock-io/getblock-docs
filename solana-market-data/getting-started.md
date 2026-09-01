---
description: >-
  This guide walks you through the basic steps required to start using Solana
  Market Data.
---

# Getting Started

### 1. Activate Solana Market Data

1. Log in to your [GetBlock account](https://account.getblock.io/)
2. Under Products, select **Solana Market Data**

<figure><img src="../.gitbook/assets/image (2).png" alt=""><figcaption></figcaption></figure>

3. To subscribe, select any payment option best for you: &#x20;

* **Card**
* **Crypto**
* **Prepaid credits**

{% hint style="success" %}
Click the **side drawer** to select your preferred payment method, then complete the purchase.

![](<../.gitbook/assets/image (3).png>)
{% endhint %}

4. Once the subscription is active, your API key will be enabled for **Solana Market Data**.

{% hint style="info" %}
Need an API key? You can create one [API Key tab](https://account.getblock.io/products/solana-data-stream#api-keys)
{% endhint %}

### 2. Select a token pair

Solana Market Data returns market information for a specific token pair.

To choose the market you want to track, provide the mint addresses for:

* **Base Mint** — the asset you want to analyze
* **Quote Mint** — the asset used to price it

For example, to receive market data for **SOL/USDC**, select the corresponding **SOL** and **USDC** mint addresses as your base and quote assets.

The selected pair is then applied across the available methods, including **Trades, Blocks, VWAP, TWAP, Volume, and Candles**.

{% hint style="info" %}
You can select the Base Mint and Quote Mint directly in the [**Playground**](https://account.getblock.io/products/solana-data-stream#playground) to explore the data for a specific pair before integrating it into your application.
{% endhint %}

### 3. Choose the data you need

**Solana Market Data** provides several levels of processed market information.

Choose the method that best matches your use case:

<table data-search="false"><thead><tr><th>Method</th><th>Best for</th></tr></thead><tbody><tr><td><strong>Trades</strong></td><td>Individual normalized trades</td></tr><tr><td><strong>Blocks</strong></td><td>Market activity aggregated by Solana slot</td></tr><tr><td><strong>VWAP</strong></td><td>Volume-weighted market price</td></tr><tr><td><strong>TWAP</strong></td><td>Time-weighted market price</td></tr><tr><td><strong>Volume</strong></td><td>Trading activity and traded volume</td></tr><tr><td><strong>Candles</strong></td><td>OHLCV charts and price visualization</td></tr><tr><td><strong>Buy/Sell Activity</strong></td><td>Comparing buying and selling activity</td></tr></tbody></table>

### 4. Select how you want to consume the data

You can access **Solana Market Data** through several protocols.

#### 1. HTTP

Use HTTP when you need a single response or want to retrieve the latest available data.

Typical use cases include:

* getting the current VWAP;
* retrieving recent trades;
* loading data when your application starts.

#### 2. WebSocket

Use WebSocket when you need continuous live updates.

It is suitable for:

* live dashboards;
* trading interfaces;
* continuously updated charts;
* real-time market monitoring.

#### 3. gRPC

Use the gRPC interface for server-side streaming and integrations that already use a Yellowstone-style workflow.

It provides processed Solana market data while allowing you to keep a familiar streaming integration model.

### 5. Configure your request

The parameters depend on the selected method, but most requests start with a few common choices:

* the **base token**;
* the **quote token**;
* the market data method you want to use;
* a **time window** for methods such as VWAP, TWAP, Volume, or Candles.

For example, you can request market data for:

```
SOL / USDC
```

and then choose whether you want Trades, VWAP, Candles, or another available metric.

### Try it in the Playground

The easiest way to explore Solana Market Data before writing code is through the **Playground**.

Use it to:

1. choose a method;
2. select a token pair;
3. configure the available parameters;
4. send a query or start a stream;
5. inspect the returned data.

The Playground uses the same concepts exposed through the API, making it useful for understanding a method before integrating it into your application.

### 6. Integrate it into your application

Once you know which data you need, use the corresponding API method from your application.

A typical integration looks like this:

```mermaid
graph TD
  A[Choose a token pair] --> B[Choose a market data method]
  B --> C[Query current or recent data]
  C --> D[Subscribe to live updates if needed]
  D --> E[Use the data in your application]
```

For exact request parameters, response fields, and streaming behavior, continue to the documentation for the specific **Market Data method** or see the **API Reference**.
