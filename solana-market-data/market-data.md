# Market data

A **Solana Market Data** subscription describes three things:&#x20;

1. the market pair to observe,
2. the type of data to receive,&#x20;
3. and how that data should be delivered.&#x20;

The `base` and `quote` mint addresses identify the pair and its direction. The `topic` selects the resulting data model, such as individual trades, **OHLCV** candles, or an aggregated price. Topics such as `ohlcv`, `twap`, `vwap`, and `volume` also use `window` to define their calculation period.

After accepting a subscription, the service can first send recent rows requested through `hydrate` and then continue streaming live changes. The `throttle` option controls the minimum interval between those updates. It does not change the aggregation window or require the client to wait until that window closes.

The example below subscribes to one-minute **OHLCV** data for the SOL/USDC pair. It requests up to ten initial rows and asks the service to push subsequent changes no more frequently than once per second. Replace the mint addresses, topic, and window to match the market data your application requires.

### Subscribe Request

Subscribe with `getblock_subscribe` and pass one structured request object:

```json
{
  "jsonrpc": "2.0",
  "id": 1,
  "method": "getblock_subscribe",
  "params": [
    {
      "source": "market",
      "topic": "ohlcv",
      "params": {
        "base": "So11111111111111111111111111111111111111112",
        "quote": "EPjFWdd5AufqSSqeM2qN1xzybapC8G4wEGGkZwyTDt1v",
        "window": "1m",
        "hydrate": 10,
        "throttle": "1s"
      }
    }
  ]
}
```

#### Market parameters

| Field     | Type    | Required                | Description                                                                            |
| --------- | ------- | ----------------------- | -------------------------------------------------------------------------------------- |
| `base`    | string  | No                      | Base58-encoded mint address of the base token.                                         |
| `quote`   | string  | No                      | Base58-encoded mint address of the quote token.                                        |
| `mint`    | string  | No                      | Base58-encoded token mint used only by the `token` topic.                              |
| `window`  | string  | Yes for windowed topics | Aggregation period.                                                                    |
| `hydrate` | integer | No                      | Number of initial rows requested when the subscription starts, from `1` through `100`. |

Accepted market windows are:

```
1s, 10s, 30s, 1m, 5m, 30m, 1h, 2h, 4h, 6h, 8h, 12h, 24h
```

`limit`, `from`, and `to` are query-only parameters and are not supported by the streaming API.

#### Optimistic delivery and reconciliation

Solana produces an ordered sequence of slots, but recently observed data can still change before the network reaches finality. Waiting for finalization before publishing every market event would add latency and could make the feed less useful for trading, monitoring, and automated execution.

Market Data therefore uses an optimistic streaming model. Rows are delivered as soon as they are available and can be corrected later through the same subscription. This gives latency-sensitive clients immediate access to new market activity while preserving a deterministic way to reconcile their local state.

The `window` parameter defines the aggregation period, not how long a client must wait for the first result. For example, a subscription with `"window": "10s"` can receive a row before the ten-second window closes. As more trades enter that window, the service sends the latest version of the row in `updates`. The `throttle` parameter controls the minimum interval between pushes and can be used to reduce the update frequency.

If a chain reorganization changes the transactions included in recent slots, affected rows can be corrected in two ways:

* `updates` contains the replacement value for an existing row;
* `deletes` contains a previously emitted row that is no longer valid on the currently observed canonical branch.

<br>

{% hint style="info" %}
\
Every changed row carries its stable `id`. Clients should maintain their local view by upserting rows from `inserts` and `updates`, then removing rows from `deletes` by `id`. A delete carries the row object, not only its identifier, so clients may also inspect or audit the removed value.

```
for (const row of [...(result.inserts ?? []), ...(result.updates ?? [])]) {
  rows.set(row.id, row)
}

for (const row of result.deletes ?? []) {
  rows.delete(row.id)
}
```
{% endhint %}

Applications that require finalized-only data can apply their own confirmation policy before treating optimistic rows as irreversible. Applications that prioritize minimum latency can act immediately, provided they also process later updates and deletes.

| Change set | Description                                                                                                  |
| ---------- | ------------------------------------------------------------------------------------------------------------ |
| `inserts`  | New rows, including the initial snapshot requested with `hydrate`.                                           |
| `updates`  | Full replacement rows for previously emitted IDs whose values changed, including active aggregation windows. |
| `deletes`  | Previously emitted rows that must be removed from the client's local state by `id`.                          |

A change-set property can be omitted when a notification contains no changes of that type. Clients should process all three categories and ignore unknown row fields for forward compatibility. Do not treat an `insert` as final merely because it was delivered first.

The Solana gateway preserves market row objects without renaming their fields. Most numeric and timestamp values are serialized as strings so clients do not lose precision. Boolean fields remain JSON booleans, and `id` can be a JSON number.

### Trades

The `trades` topic streams normalized individual trades for the selected pair. Use it when an application needs transaction-level activity or must calculate its own prices, indicators, and aggregations.

#### Response fields

<table data-search="false"><thead><tr><th>Field</th><th>Description</th></tr></thead><tbody><tr><td><code>id</code></td><td>Row identifier.</td></tr><tr><td><code>signature</code></td><td>Solana transaction signature.</td></tr><tr><td><code>signer</code></td><td>Wallet that signed the transaction.</td></tr><tr><td><code>slot</code></td><td>Solana slot in which the trade was observed.</td></tr><tr><td><code>timestamp</code></td><td>Trade timestamp in ISO 8601 format.</td></tr><tr><td><code>base</code></td><td>Mint address of the base token.</td></tr><tr><td><code>quote</code></td><td>Mint address of the quote token.</td></tr><tr><td><code>base_amount</code></td><td>Base-token amount in the token's smallest units.</td></tr><tr><td><code>quote_amount</code></td><td>Quote-token amount in the token's smallest units.</td></tr><tr><td><code>base_decimals</code></td><td>Decimal precision of the base token.</td></tr><tr><td><code>quote_decimals</code></td><td>Decimal precision of the quote token.</td></tr><tr><td><code>base_volume</code></td><td>Human-readable amount of the base token exchanged.</td></tr><tr><td><code>quote_volume</code></td><td>Human-readable amount of the quote token exchanged.</td></tr><tr><td><code>price</code></td><td>Trade price expressed in the quote token.</td></tr><tr><td><code>is_buy</code></td><td><code>true</code> when the observed trade is classified as a buy.</td></tr></tbody></table>

### Block

The `block` topic aggregates observed trading activity for the selected pair within one Solana slot. It provides a compact view of short-term activity without requiring clients to process every trade.

#### Response fields

| Field          | Description                                                 |
| -------------- | ----------------------------------------------------------- |
| `id`           | Row identifier.                                             |
| `slot`         | Solana slot covered by the aggregate.                       |
| `timestamp`    | Timestamp of the latest activity included in the aggregate. |
| `base`         | Mint address of the base token.                             |
| `quote`        | Mint address of the quote token.                            |
| `vwap`         | Volume-weighted average price for the slot.                 |
| `min`          | Lowest observed price in the slot.                          |
| `max`          | Highest observed price in the slot.                         |
| `base_volume`  | Total volume denominated in the base token.                 |
| `quote_volume` | Total volume denominated in the quote token.                |
| `total_volume` | Total trading volume for the slot.                          |
| `buy_volume`   | Volume attributed to buy swaps.                             |
| `sell_volume`  | Volume attributed to sell swaps.                            |
| `num_swaps`    | Total number of observed swaps.                             |
| `buy_swaps`    | Number of buy swaps.                                        |
| `sell_swaps`   | Number of sell swaps.                                       |

### OHLCV

The `ohlcv` topic provides ready-to-use candles for charts, token pages, dashboards, alerts, and other time-based market visualizations.

#### Response fields

| Field             | Description                                       |
| ----------------- | ------------------------------------------------- |
| `id`              | Candle identifier.                                |
| `base`            | Mint address of the base token.                   |
| `quote`           | Mint address of the quote token.                  |
| `timestamp`       | Timestamp associated with the latest candle data. |
| `window_duration` | Aggregation period used to build the candle.      |
| `open`            | First observed price in the window.               |
| `high`            | Highest observed price in the window.             |
| `low`             | Lowest observed price in the window.              |
| `close`           | Latest observed price in the window.              |
| `base_volume`     | Trading volume denominated in the base token.     |
| `quote_volume`    | Trading volume denominated in the quote token.    |
| `volume_usd`      | Trading volume expressed in USD.                  |
| `data_points`     | Number of observations included in the candle.    |
| `window_start`    | Start of the aggregation window.                  |
| `window_end`      | End of the aggregation window.                    |

### TWAP

The `twap` topic provides the time-weighted average price over the selected window. It gives equal weight to each period of time and is useful for smoother price monitoring and time-based execution benchmarks.

#### Response fields

| Field             | Description                                               |
| ----------------- | --------------------------------------------------------- |
| `id`              | Row identifier.                                           |
| `base`            | Mint address of the base token.                           |
| `quote`           | Mint address of the quote token.                          |
| `timestamp`       | Timestamp of the latest data included in the calculation. |
| `slot`            | Latest Solana slot included in the calculation.           |
| `window_duration` | Aggregation period used for the calculation.              |
| `twap`            | Time-weighted average price in the quote token.           |
| `twap_usd`        | Time-weighted average price expressed in USD.             |
| `current_price`   | Latest observed price for comparison with the average.    |
| `data_points`     | Number of observations included in the calculation.       |
| `window_start`    | Start of the calculation window.                          |
| `window_end`      | End of the calculation window.                            |

### VWAP

The `vwap` topic provides the volume-weighted average price over the selected window. Trades with more volume have more influence on the result, making VWAP useful for execution benchmarks and liquidity-aware market monitoring.

#### Response fields

| Field             | Description                                               |
| ----------------- | --------------------------------------------------------- |
| `id`              | Row identifier.                                           |
| `base`            | Mint address of the base token.                           |
| `quote`           | Mint address of the quote token.                          |
| `timestamp`       | Timestamp of the latest data included in the calculation. |
| `slot`            | Latest Solana slot included in the calculation.           |
| `window_duration` | Aggregation period used for the calculation.              |
| `vwap`            | Volume-weighted average price in the quote token.         |
| `vwap_usd`        | Volume-weighted average price expressed in USD.           |
| `total_volume`    | Volume included in the calculation.                       |
| `data_points`     | Number of observations included in the calculation.       |
| `window_start`    | Start of the calculation window.                          |
| `window_end`      | End of the calculation window.                            |

### Volume

The `volume` topic summarizes trading activity for the selected pair and window. Use it to monitor market participation, detect changes in activity, or build volume-based indicators.

#### Response fields

| Field             | Description                                                 |
| ----------------- | ----------------------------------------------------------- |
| `id`              | Row identifier.                                             |
| `base`            | Mint address of the base token.                             |
| `quote`           | Mint address of the quote token.                            |
| `timestamp`       | Timestamp of the latest activity included in the aggregate. |
| `window_duration` | Aggregation period used for the calculation.                |
| `base_volume`     | Total volume denominated in the base token.                 |
| `quote_volume`    | Total volume denominated in the quote token.                |
| `buy_volume`      | Volume attributed to buy swaps.                             |
| `sell_volume`     | Volume attributed to sell swaps.                            |
| `total_volume`    | Total trading volume in the window.                         |
| `total_swaps`     | Total number of swaps in the window.                        |
| `window_start`    | Start of the aggregation window.                            |
| `window_end`      | End of the aggregation window.                              |

#### Buy and sell pressure

Buy/sell pressure is not a separate public topic in the current API. Derive it from `buy_volume` and `sell_volume` returned by `volume`, or use the equivalent fields from `block` for slot-level analysis. For example:

```
pressure = (buy_volume - sell_volume) / (buy_volume + sell_volume)
```

Applications must handle a zero total volume before calculating the ratio.

### Token

The `token` topic streams metadata for a token selected with the `mint` parameter. It does not use `base`, `quote`, or `window`.

#### Response fields

| Field      | Description                     |
| ---------- | ------------------------------- |
| `id`       | Row identifier.                 |
| `mint`     | Token mint address.             |
| `name`     | Token name.                     |
| `symbol`   | Token ticker symbol.            |
| `decimals` | Decimal precision of the token. |
| `supply`   | Current token supply.           |
