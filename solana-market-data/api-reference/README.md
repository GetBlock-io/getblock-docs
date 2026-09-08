---
description: >-
  Full reference for the Solana Market Data streaming API: methods, topics,
  parameters, and response fields, with examples in JavaScript and Python.
---

# API Reference

This section documents every method and topic exposed by the Solana Market Data streaming API.

The API has just two methods — [`getblock_subscribe`](getblock_subscribe-market-data.md) and [`getblock_unsubscribe`](getblock_unsubscribe-market-data.md). What you receive is decided by the **topic** you subscribe to. Each topic has its own page below covering its parameters, a live sample response, and every field it returns.

## Endpoint

{% code overflow="wrap" %}
```
wss://stream.eu-central-1.getblock.io/v1/solana-mainnet/stream?apiKey=<API-KEY>
```
{% endcode %}

The API key goes in the `apiKey` query parameter, not in a header. Create one in [Dashboard → API Keys](https://account.getblock.io/products/solana-data-stream#api-keys) and confirm Solana Market Data is activated on it.

## Methods

| Method                                                        | Description                                                          |
| ------------------------------------------------------------- | ---------------------------------------------------------------------- |
| [`getblock_subscribe`](getblock_subscribe-market-data.md)     | Open a subscription. Returns a subscription ID, then streams change sets. |
| [`getblock_unsubscribe`](getblock_unsubscribe-market-data.md) | Cancel one subscription without closing the socket.                  |

## Topics

| Topic                                     | Description                                        | Requires   |
| ----------------------------------------- | -------------------------------------------------- | ---------- |
| [`trades`](trades-market-data.md)         | Normalized individual trades                       | —          |
| [`block`](block-market-data.md)           | Trading activity aggregated for one Solana slot    | —          |
| [`ohlcv`](ohlcv-market-data.md)           | Open, high, low, close, and volume candles         | `window`   |
| [`twap`](twap-market-data.md)             | Time-weighted average price                        | `window`   |
| [`vwap`](vwap-market-data.md)             | Volume-weighted average price                      | `window`   |
| [`volume`](volume-market-data.md)         | Trading volume and buy/sell activity               | `window`   |
| [`token`](token-market-data.md)           | Token metadata                                     | `mint`     |

{% hint style="danger" %}
The [`token`](token-market-data.md) topic is currently not returning data — a subscription to it receives no acknowledgement, no data, and no error. See its page for details and a workaround.
{% endhint %}

## Choosing a topic

| If you need                                          | Use        |
| ---------------------------------------------------- | ---------- |
| Individual trades and maximum calculation flexibility | `trades`   |
| Market activity for each Solana slot                  | `block`    |
| Candles or price charts                               | `ohlcv`    |
| A price weighted by elapsed time                      | `twap`     |
| A price weighted by traded volume                     | `vwap`     |
| Buy, sell, and total market activity                  | `volume`   |
| Token metadata                                        | `token`    |

## Quickstart

Subscribe to one-minute SOL/USDC candles, seeded with the last 10 and updated at most once a second:

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

## Concepts that apply to every topic

**The notification envelope.** Row data is nested under `params.result`, not `result`. Only the initial acknowledgement uses a top-level `result`, and it is a plain string — the subscription ID.

**Change sets.** Every notification carries up to three arrays. Maintain local state by upserting `inserts` and `updates` by `id`, then removing `deletes` by `id`. A property can be omitted when there are no changes of that type.

| Change set | Description                                                                     |
| ---------- | --------------------------------------------------------------------------------- |
| `inserts`  | New rows, including the initial `hydrate` snapshot.                             |
| `updates`  | Full replacement rows for previously emitted IDs whose values changed.          |
| `deletes`  | Rows to remove from local state by `id`. Carries the whole row, not just the ID. |

**`hydrate` sets the row-set size.** It is not only the size of the opening snapshot — the subscription holds the set at that size, evicting the oldest rows through `deletes` as new ones arrive.

{% hint style="warning" %}
A row in `deletes` is not by itself evidence of a chain reorganization. On fast-moving topics such as `trades` and `block`, most deletes are ordinary eviction from the `hydrate` window.
{% endhint %}

**Optimistic delivery.** Rows are published before finality and revised in place. An `insert` is not final because it arrived first; the active window is re-sent through `updates` under the same `id`.

**Serialization.** Numeric and timestamp values are strings, to preserve precision. `id` is a JSON number, booleans are JSON booleans, and durations use ISO 8601 format (`1m` comes back as `PT1M`). Ignore unknown fields for forward compatibility.

**One socket, many subscriptions.** Send several `getblock_subscribe` requests on the same connection and route notifications on `params.subscription`.
