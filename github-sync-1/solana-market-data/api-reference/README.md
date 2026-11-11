---
description: >-
  Full reference for the Solana Market Data streaming API: methods, topics,
  parameters, and response fields, with examples in JavaScript and Python.
---

# API Reference

This section documents every method and topic exposed by the Solana Market Data streaming API.

The API has just two methods — [`getblock_subscribe`](getblock_subscribe-market-data.md) and [`getblock_unsubscribe`](getblock_unsubscribe-market-data.md). What you receive is decided by the [**topic**](./#topics) you subscribe to. Each topic has its own page below covering its parameters, a live sample response, and every field it returns.

### Quickstart

Subscribe to one-minute SOL/USDC candles, seeded with the last 10 and updated at most once a second:

{% tabs %}
{% tab title="Request" %}
{% code overflow="wrap" %}
```json
{
  "jsonrpc": "2.0",
  "id": "getblock.io",
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
{% endcode %}
{% endtab %}

{% tab title="Response" %}
The service replies with a subscription ID, then pushes change sets:

```json
{
  "jsonrpc": "2.0",
  "method": "getblock_subscribe",
  "params": {
    "subscription": "0x4503d281ca474f3322ed277fde75db15",
    "result": { "inserts": [ … ], "updates": [ … ], "deletes": [ … ] }
  }
}
```
{% endtab %}
{% endtabs %}

### Endpoint

{% code overflow="wrap" %}
```bash
wss://stream.eu-central-1.getblock.io/v1/solana-mainnet/stream?apiKey=<API-KEY>
```
{% endcode %}

The API key goes in the `apiKey` query parameter, not in a header. Create one in [Dashboard → API Keys](https://account.getblock.io/products/solana-data-stream#api-keys) and confirm Solana Market Data is activated on it.

### Methods

| Method                                                        | Description                                                               |
| ------------------------------------------------------------- | ------------------------------------------------------------------------- |
| [`getblock_subscribe`](getblock_subscribe-market-data.md)     | Open a subscription. Returns a subscription ID, then streams change sets. |
| [`getblock_unsubscribe`](getblock_unsubscribe-market-data.md) | Cancel one subscription without closing the socket.                       |

### Topics

<table data-search="false"><thead><tr><th>Topic</th><th>Description</th><th>Requires</th></tr></thead><tbody><tr><td><a href="trades-market-data.md"><code>trades</code></a></td><td>Normalized individual trades</td><td>—</td></tr><tr><td><a href="block-market-data.md"><code>block</code></a></td><td>Trading activity aggregated for one Solana slot</td><td>—</td></tr><tr><td><a href="ohlcv-market-data.md"><code>ohlcv</code></a></td><td>Open, high, low, close, and volume candles</td><td><code>window</code></td></tr><tr><td><a href="twap-market-data.md"><code>twap</code></a></td><td>Time-weighted average price</td><td><code>window</code></td></tr><tr><td><a href="vwap-market-data.md"><code>vwap</code></a></td><td>Volume-weighted average price</td><td><code>window</code></td></tr><tr><td><a href="volume-market-data.md"><code>volume</code></a></td><td>Trading volume and buy/sell activity</td><td><code>window</code></td></tr><tr><td><a href="token-market-data.md"><code>token</code></a></td><td>Token metadata</td><td><code>mint</code></td></tr></tbody></table>

{% hint style="danger" %}
The [`token`](token-market-data.md) topic is currently not returning data — a subscription to it receives no acknowledgement, no data, and no error. See its page for details and a workaround.
{% endhint %}

### Choosing a topic

<table data-search="false"><thead><tr><th>If you need</th><th>Use</th></tr></thead><tbody><tr><td>Individual trades and maximum calculation flexibility</td><td><code>trades</code></td></tr><tr><td>Market activity for each Solana slot</td><td><code>block</code></td></tr><tr><td>Candles or price charts</td><td><code>ohlcv</code></td></tr><tr><td>A price weighted by elapsed time</td><td><code>twap</code></td></tr><tr><td>A price weighted by traded volume</td><td><code>vwap</code></td></tr><tr><td>Buy, sell, and total market activity</td><td><code>volume</code></td></tr><tr><td>Token metadata</td><td><code>token</code></td></tr></tbody></table>

### Concepts that apply to every topic

1. **The notification envelope:** Row data is nested under `params.result`, not `result`. Only the initial acknowledgment uses a top-level `result`, and it is a plain string — the subscription ID.
2. **Change sets:** Each notification can include up to three arrays. Maintain local state by upserting `inserts` and `updates` by `id`, then removing `deletes` by `id`. A property can be omitted when there are no changes of that type.

| Change set | Description                                                                      |
| ---------- | -------------------------------------------------------------------------------- |
| `inserts`  | New rows, including the initial `hydrate` snapshot.                              |
| `updates`  | Full replacement rows for previously emitted IDs whose values changed.           |
| `deletes`  | Rows to remove from local state by `id`. Carries the whole row, not just the ID. |

3. **`hydrate` sets the row-set size.** It is not only the size of the opening snapshot — the subscription holds the set at that size, evicting the oldest rows through `deletes` as new ones arrive.

{% hint style="warning" %}
A row in `deletes` is not by itself evidence of a chain reorganization. On fast-moving topics such as `trades` and `block`, most deletes are ordinary eviction from the `hydrate` window.
{% endhint %}

4. **Optimistic delivery:** Rows are published before finality and revised in place. An `insert` is not final because it arrived first; the active window is re-sent through `updates` under the same `id`.
5. **Serialization:** Numeric and timestamp values are strings, to preserve precision. `id` is a JSON number, booleans are JSON booleans, and durations use ISO 8601 format (`1m` comes back as `PT1M`). Ignore unknown fields for forward compatibility.
6. **One socket, many subscriptions:** Send several `getblock_subscribe` requests on the same connection and route notifications on `params.subscription`.
