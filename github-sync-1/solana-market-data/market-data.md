---
description: >-
  How a Solana Market Data subscription works — choosing a pair, a topic, and
  how the data is delivered.
---

# Market Data

A **Solana Market Data** subscription describes three things:

1. the market pair to observe,
2. the type of data to receive,
3. and how that data should be delivered.

This page explains those three choices. For request and response formats, parameter tables, and the fields each topic returns, see the [**API Reference**](api-reference/).

### 1. The market pair

The `base` and `quote` mint addresses identify the pair and its direction. For SOL priced in USDC, `base` is the SOL mint and `quote` is the USDC mint.

{% hint style="info" %}
Both are optional, but omitting them does not select a default pair — it subscribes you to **every market**, a high-volume firehose spanning hundreds of pairs. Set both unless you genuinely want all markets.
{% endhint %}

The `token` topic is the exception: it tracks a single token through a `mint` parameter and does not use a pair.

### 2. The type of data

The `topic` selects the data model you receive — individual trades, OHLCV candles, an aggregated price, and so on.

<table data-search="false"><thead><tr><th>Topic</th><th>Use it for</th><th>Requires</th></tr></thead><tbody><tr><td><a href="api-reference/trades-market-data.md"><code>trades</code></a></td><td>Individual trades and maximum calculation flexibility</td><td>—</td></tr><tr><td><a href="api-reference/block-market-data.md"><code>block</code></a></td><td>Market activity for each Solana slot</td><td>—</td></tr><tr><td><a href="api-reference/ohlcv-market-data.md"><code>ohlcv</code></a></td><td>Candles or price charts</td><td><code>window</code></td></tr><tr><td><a href="api-reference/twap-market-data.md"><code>twap</code></a></td><td>A price weighted by elapsed time</td><td><code>window</code></td></tr><tr><td><a href="api-reference/vwap-market-data.md"><code>vwap</code></a></td><td>A price weighted by traded volume</td><td><code>window</code></td></tr><tr><td><a href="api-reference/volume-market-data.md"><code>volume</code></a></td><td>Buy, sell, and total market activity</td><td><code>window</code></td></tr><tr><td><a href="api-reference/token-market-data.md"><code>token</code></a></td><td>Token metadata</td><td><code>mint</code></td></tr></tbody></table>

Choosing between them is mostly a question of how much processing you want to do yourself. `trades` gives you raw fills to aggregate however you like; the windowed topics hand you a metric that is already calculated.

#### Aggregation windows

Topics such as `ohlcv`, `twap`, `vwap`, and `volume` use `window` to define their calculation period:

```
1s, 10s, 30s, 1m, 5m, 30m, 1h, 2h, 4h, 6h, 8h, 12h, 24h
```

`window` sets the aggregation period only. It does not decide how long you wait for a first result — see [Delivery](market-data.md#3-delivery) below.

### 3. Delivery

After accepting a subscription, the service can first send recent rows requested through `hydrate`, then continue streaming live changes. `throttle` controls the minimum interval between those updates.

{% hint style="warning" %}
`hydrate` is not only an opening snapshot. It sets the size of the row set the subscription maintains: the service sends that many rows up front and then maintainsholds the set at that size, evicting the oldest rows as new ones arrive. Size it to the depth of history you actually want to keep.
{% endhint %}

### Optimistic delivery

Solana produces an ordered sequence of slots, but recently observed data can still change before the network reaches finality. Waiting for finalization before publishing every market event would add latency and could make the feed less useful for trading, monitoring, and automated execution.

Market Data therefore uses an optimistic streaming model. Rows are delivered as soon as they are available and can be corrected later through the same subscription. This gives latency-sensitive clients immediate access to new market activity while preserving a deterministic means of reconciling their local state.

In practice this means a subscription with `"window": "10s"` can receive a row before the ten-second window closes. As more trades enter that window, the service sends the latest version of the row — under the same `id` — rather than a new one.

Applications that require finalized-only data can apply their own confirmation policy before treating optimistic rows as irreversible. Applications that prioritize minimum latency can act immediately, provided they also process later corrections.

### Keeping local state correct

Every row carries a stable `id`, and each notification carries up to three change sets: `inserts`, `updates`, and `deletes`. Maintain your local view by upserting rows from `inserts` and `updates`, then removing rows from `deletes` by `id`.

{% hint style="warning" %}
A row in `deletes` is **not** by itself evidence of a chain reorganization. On fast-moving topics such as `trades` and `block`, most deletes are ordinary eviction from the `hydrate` window. Reconcile by `id` rather than treating a delete as a reorg signal.
{% endhint %}

The exact envelope, the change-set semantics, and a reconciliation snippet are covered in [`getblock_subscribe`](api-reference/getblock_subscribe-market-data.md).

## Next steps

* [**API Reference**](api-reference/) — methods, parameters, and every response field
* [**Getting Started**](getting-started.md) — activating the product and choosing a pair
* [**Playground**](https://account.getblock.io/products/solana-data-stream#playground) — try a topic before integrating it
