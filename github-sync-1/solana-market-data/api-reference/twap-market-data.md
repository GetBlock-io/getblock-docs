---
description: >-
  Stream the time-weighted average price for a Solana token pair. Complete
  guide on how to use the twap topic in GetBlock Solana Market Data
  documentation.
---

# twap - Solana Market Data

The `twap` topic provides the time-weighted average price over the selected window. It gives equal weight to each period of time regardless of how much volume traded, which makes it resistant to short volume spikes and useful for smoother price monitoring and time-based execution benchmarks.

{% hint style="warning" %}
**WebSocket-only topic.** Subscribe with `getblock_subscribe` over `wss://stream.eu-central-1.getblock.io/v1/solana-mainnet/stream`. The `limit`, `from`, and `to` query parameters are not supported by the streaming API.
{% endhint %}

## Parameters

Pass one structured request object to [`getblock_subscribe`](getblock_subscribe-market-data.md), with `"source": "market"` and `"topic": "twap"`. The fields below go inside the nested `params` object.

| Parameter | Type | Required | Description |
| --- | --- | --- | --- |
| `base` | string | No | Base58-encoded mint address of the base token. Omit to receive every market. |
| `quote` | string | No | Base58-encoded mint address of the quote token. Omit to receive every market. |
| `window` | string | Yes | Aggregation period. One of `1s`, `10s`, `30s`, `1m`, `5m`, `30m`, `1h`, `2h`, `4h`, `6h`, `8h`, `12h`, `24h`. |
| `hydrate` | integer | No | Size of the row set to maintain, `1`–`100`. Sent as an initial snapshot, then held at this size (see [Row set behavior](#row-set-behavior)). |
| `throttle` | string | No | Minimum interval between pushes. A positive integer followed by `ms`, `s`, `m`, or `h` (for example `1s`). |

{% hint style="info" %}
While a window is still open, `total_duration` is shorter than `window_duration` — the average covers only the elapsed portion. Compare the two if you need to know whether a row represents a complete window.
{% endhint %}

## Request Example

{% tabs %}
{% tab title="wscat" %}
{% code overflow="wrap" %}
```bash
# WebSocket-only. Connect first:
wscat -c 'wss://stream.eu-central-1.getblock.io/v1/solana-mainnet/stream?apiKey=<API-KEY>'

# Then send:
{"jsonrpc":"2.0","id":"getblock.io","method":"getblock_subscribe","params":[{"source":"market","topic":"twap","params":{"base":"So11111111111111111111111111111111111111112","quote":"EPjFWdd5AufqSSqeM2qN1xzybapC8G4wEGGkZwyTDt1v","window":"5m","hydrate":5,"throttle":"1s"}}]}
```
{% endcode %}
{% endtab %}

{% tab title="JavaScript" %}
```javascript
// Node 20+ has a global WebSocket; no dependency required.
const ws = new WebSocket('wss://stream.eu-central-1.getblock.io/v1/solana-mainnet/stream?apiKey=<API-KEY>');
const rows = new Map();

ws.onopen = () => {
  ws.send(JSON.stringify({
    "jsonrpc": "2.0",
    "id": "getblock.io",
    "method": "getblock_subscribe",
    "params": [
      {
        "source": "market",
        "topic": "twap",
        "params": {
          "base": "So11111111111111111111111111111111111111112",
          "quote": "EPjFWdd5AufqSSqeM2qN1xzybapC8G4wEGGkZwyTDt1v",
          "window": "5m",
          "hydrate": 5,
          "throttle": "1s"
        }
      }
    ]
  }));
};

ws.onmessage = (event) => {
  const msg = JSON.parse(event.data);

  // First reply is the subscription ID
  if (typeof msg.result === 'string') {
    console.log('Subscribed:', msg.result);
    return;
  }

  // Every later message is a change set
  const { inserts = [], updates = [], deletes = [] } = msg.params.result;
  for (const row of [...inserts, ...updates]) rows.set(row.id, row);
  for (const row of deletes) rows.delete(row.id);
};
```
{% endtab %}

{% tab title="Python" %}
```python
import asyncio, json, websockets

async def main():
    async with websockets.connect('wss://stream.eu-central-1.getblock.io/v1/solana-mainnet/stream?apiKey=<API-KEY>') as ws:
        await ws.send(json.dumps({
          "jsonrpc": "2.0",
          "id": "getblock.io",
          "method": "getblock_subscribe",
          "params": [
            {
              "source": "market",
              "topic": "twap",
              "params": {
                "base": "So11111111111111111111111111111111111111112",
                "quote": "EPjFWdd5AufqSSqeM2qN1xzybapC8G4wEGGkZwyTDt1v",
                "window": "5m",
                "hydrate": 5,
                "throttle": "1s"
              }
            }
          ]
        }))

        rows = {}
        async for message in ws:
            msg = json.loads(message)

            # First reply is the subscription ID
            if isinstance(msg.get('result'), str):
                print('Subscribed:', msg['result'])
                continue

            result = msg['params']['result']
            for row in result.get('inserts', []) + result.get('updates', []):
                rows[row['id']] = row
            for row in result.get('deletes', []):
                rows.pop(row['id'], None)

asyncio.run(main())
```
{% endtab %}
{% endtabs %}

## Response Example

The first reply is the subscription ID:

```json
{
  "jsonrpc": "2.0",
  "id": "getblock.io",
  "result": "0x4503d281ca474f3322ed277fde75db15"
}
```

Every later message is a change-set notification:

```json
{
  "jsonrpc": "2.0",
  "method": "getblock_subscribe",
  "params": {
    "subscription": "0x4503d281ca474f3322ed277fde75db15",
    "result": {
      "inserts": [
        {
          "base": "So11111111111111111111111111111111111111112",
          "cumulative_weighted_sum": "3730.0708443739",
          "cumulative_weighted_sum_usd": "3730.0708443739",
          "current_price": "103.625528855319",
          "current_price_usd": "103.625528855319",
          "data_points": "75",
          "id": 856191,
          "quote": "EPjFWdd5AufqSSqeM2qN1xzybapC8G4wEGGkZwyTDt1v",
          "slot": "445316622",
          "timestamp": "2026-09-08T10:33:36.000000000Z",
          "total_duration": "PT36S",
          "twap": "103.613079010386",
          "twap_usd": "103.613079010386",
          "window_duration": "PT1M",
          "window_end": "2026-09-08T10:34:00.000000000Z",
          "window_start": "2026-09-08T10:33:00.000000000Z"
        }
      ],
      "updates": [],
      "deletes": []
    }
  }
}
```

## Response Parameters

Each object in `inserts`, `updates`, and `deletes` has the following shape.

| Field | Type | Description |
| --- | --- | --- |
| `base` | string | Mint address of the base token. |
| `cumulative_weighted_sum` | string | Running sum of price × elapsed time used to derive `twap`. |
| `cumulative_weighted_sum_usd` | string | Running sum of USD price × elapsed time used to derive `twap_usd`. |
| `current_price` | string | Latest observed price, for comparison with the average. |
| `current_price_usd` | string | Latest observed price expressed in USD. |
| `data_points` | string | Number of observations included in the row. |
| `id` | number | Row identifier. Stable across `updates` and `deletes` — use it as the key for local state. |
| `quote` | string | Mint address of the quote token. |
| `slot` | string | Latest Solana slot included in the row. |
| `timestamp` | string | Timestamp of the latest data included in the row. |
| `total_duration` | string | Time elapsed so far within the window, as an ISO 8601 duration. Shorter than `window_duration` while the window is still open. |
| `twap` | string | Time-weighted average price in the quote token. |
| `twap_usd` | string | Time-weighted average price expressed in USD. |
| `window_duration` | string | Aggregation period used to build the row, as an ISO 8601 duration (`PT1M` for a `1m` window). |
| `window_end` | string | End of the aggregation window. |
| `window_start` | string | Start of the aggregation window. |

{% hint style="info" %}
Numeric and timestamp values are serialized as **strings** to preserve precision. `id` is a JSON number and boolean fields are JSON booleans. Timestamps are ISO 8601 with nanosecond precision; durations such as `window_duration` use ISO 8601 duration format (`1m` is returned as `PT1M`). Ignore unknown fields for forward compatibility.
{% endhint %}

## Row set behavior

`hydrate` sets the size of the row set the subscription maintains — not merely the size of the opening snapshot. The service sends that many rows up front, then keeps the set at that size for the lifetime of the subscription: as new rows arrive, the oldest are emitted in `deletes`.

{% hint style="warning" %}
A row in `deletes` is **not** by itself evidence of a chain reorganization. On fast-moving topics most deletes are ordinary eviction from the `hydrate` window. Reconcile by `id` and do not treat a delete as a reorg signal.
{% endhint %}

Maintain local state by upserting `inserts` and `updates` by `id`, then removing `deletes` by `id`:

```javascript
for (const row of [...(result.inserts ?? []), ...(result.updates ?? [])]) {
  rows.set(row.id, row)
}

for (const row of result.deletes ?? []) {
  rows.delete(row.id)
}
```

## Use Cases

* Time-based execution benchmarks for order routing
* Smoothed price displays that should not react to a single large fill
* Oracle-style reference pricing over a fixed interval
* Comparing `current_price` against a time-weighted baseline

## Error Handling

| Code | Message | Cause |
| --- | --- | --- |
| `-32602` | `unsupported source "…": supported sources are "market" and "priorityfee"` | `source` is not a recognized value. |
| `-32602` | `unsupported topic: supported topics are trades, ohlcv, block, twap, vwap, volume, token` | `topic` is not a recognized value. |
| `-32602` | `window must be one of 1s, 10s, 30s, 1m, …` | `window` is missing or not an accepted value on a windowed topic. |
| `-32602` | `hydrate must be an integer from 1 to 100` | `hydrate` is outside the accepted range. |
| `-32602` | `throttle must be a positive integer followed by ms, s, m, or h` | `throttle` is malformed. |
| `-32602` | `base must be a base58-encoded 32-byte Solana public key` | `base` or `quote` is not a valid mint address. |
| HTTP `401` | `authorization failed` | API key missing, invalid, or Solana Market Data is not activated on it. WebSocket clients cannot read the handshake body and surface this as close code `1006`. |
