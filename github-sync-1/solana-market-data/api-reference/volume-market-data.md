---
description: >-
  Stream trading volume and buy/sell activity for a Solana token pair.
  Complete guide on how to use the volume topic in GetBlock Solana Market Data
  documentation.
---

# volume - Solana Market Data

The `volume` topic summarizes trading activity for the selected pair and window: total traded volume, the split between buy and sell swaps, and swap counts. Use it to monitor market participation, detect changes in activity, or build volume-based indicators.

{% hint style="warning" %}
**WebSocket-only topic.** Subscribe with `getblock_subscribe` over `wss://stream.eu-central-1.getblock.io/v1/solana-mainnet/stream`. The `limit`, `from`, and `to` query parameters are not supported by the streaming API.
{% endhint %}

## Parameters

Pass one structured request object to [`getblock_subscribe`](getblock_subscribe-market-data.md), with `"source": "market"` and `"topic": "volume"`. The fields below go inside the nested `params` object.

| Parameter | Type | Required | Description |
| --- | --- | --- | --- |
| `base` | string | No | Base58-encoded mint address of the base token. Omit to receive every market. |
| `quote` | string | No | Base58-encoded mint address of the quote token. Omit to receive every market. |
| `window` | string | Yes | Aggregation period. One of `1s`, `10s`, `30s`, `1m`, `5m`, `30m`, `1h`, `2h`, `4h`, `6h`, `8h`, `12h`, `24h`. |
| `hydrate` | integer | No | Size of the row set to maintain, `1`–`100`. Sent as an initial snapshot, then held at this size (see [Row set behavior](#row-set-behavior)). |
| `throttle` | string | No | Minimum interval between pushes. A positive integer followed by `ms`, `s`, `m`, or `h` (for example `1s`). |

### Buy and sell pressure

Buy/sell pressure is not a separate topic. The `volume` topic returns `buy_sell_ratio` directly — values above `1` indicate net buying pressure — so you do not need to derive it yourself. If you need the same signal at slot granularity, compute it from the `buy_volume` and `sell_volume` fields of the [`block`](block-market-data.md) topic:

```
pressure = (buy_volume - sell_volume) / (buy_volume + sell_volume)
```

Handle a zero total volume before calculating the ratio.

## Request Example

{% tabs %}
{% tab title="wscat" %}
{% code overflow="wrap" %}
```bash
# WebSocket-only. Connect first:
wscat -c 'wss://stream.eu-central-1.getblock.io/v1/solana-mainnet/stream?apiKey=<API-KEY>'

# Then send:
{"jsonrpc":"2.0","id":"getblock.io","method":"getblock_subscribe","params":[{"source":"market","topic":"volume","params":{"base":"So11111111111111111111111111111111111111112","quote":"EPjFWdd5AufqSSqeM2qN1xzybapC8G4wEGGkZwyTDt1v","window":"5m","hydrate":5,"throttle":"1s"}}]}
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
        "topic": "volume",
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
              "topic": "volume",
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
          "base_volume": "908.42465659",
          "buy_sell_ratio": "0.9177293",
          "buy_swaps": "160",
          "buy_volume": "434.726601127",
          "buy_volume_usd": "45050.9991123185",
          "data_points": "86",
          "duration": "PT1M",
          "id": 856191,
          "quote": "EPjFWdd5AufqSSqeM2qN1xzybapC8G4wEGGkZwyTDt1v",
          "quote_volume": "94132.689141",
          "sell_swaps": "157",
          "sell_volume": "473.698055463",
          "sell_volume_usd": "49081.6917744601",
          "timestamp": "2026-09-08T10:34:00.000000000Z",
          "total_swaps": "317",
          "total_volume": "908.42465659",
          "total_volume_usd": "94132.6908867786",
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
| `base_volume` | string | Total volume denominated in the base token. |
| `buy_sell_ratio` | string | Ratio of buy volume to sell volume. Values above `1` indicate net buying pressure. |
| `buy_swaps` | string | Number of buy swaps in the window. |
| `buy_volume` | string | Volume attributed to buy swaps. |
| `buy_volume_usd` | string | Buy-swap volume expressed in USD. |
| `data_points` | string | Number of aggregation intervals included in the row. |
| `duration` | string | Aggregation period, as an ISO 8601 duration. Duplicates `window_duration`. |
| `id` | number | Row identifier. Stable across `updates` and `deletes` — use it as the key for local state. |
| `quote` | string | Mint address of the quote token. |
| `quote_volume` | string | Total volume denominated in the quote token. |
| `sell_swaps` | string | Number of sell swaps in the window. |
| `sell_volume` | string | Volume attributed to sell swaps. |
| `sell_volume_usd` | string | Sell-swap volume expressed in USD. |
| `timestamp` | string | Timestamp of the latest activity included in the aggregate. |
| `total_swaps` | string | Total number of swaps in the window. |
| `total_volume` | string | Total trading volume in the window. |
| `total_volume_usd` | string | Total trading volume expressed in USD. |
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

* Volume bars beneath a price chart
* Monitoring market participation and liquidity changes
* Buy/sell pressure indicators via `buy_sell_ratio`
* Activity-spike alerting on `total_swaps` or `total_volume`

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
