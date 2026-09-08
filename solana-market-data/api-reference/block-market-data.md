---
description: >-
  Stream Solana trading activity aggregated per slot. Complete guide on how to
  use the block topic in GetBlock Solana Market Data documentation.
---

# block - Solana Market Data

The `block` topic aggregates observed trading activity for the selected pair within a single Solana slot. It gives a compact view of short-term activity without requiring your application to process every individual trade, and it is the finest-grained aggregate the API offers.

{% hint style="warning" %}
**WebSocket-only topic.** Subscribe with `getblock_subscribe` over `wss://stream.eu-central-1.getblock.io/v1/solana-mainnet/stream`. The `limit`, `from`, and `to` query parameters are not supported by the streaming API.
{% endhint %}

## Parameters

Pass one structured request object to [`getblock_subscribe`](getblock_subscribe-market-data.md), with `"source": "market"` and `"topic": "block"`. The fields below go inside the nested `params` object.

| Parameter | Type | Required | Description |
| --- | --- | --- | --- |
| `base` | string | No | Base58-encoded mint address of the base token. Omit to receive every market. |
| `quote` | string | No | Base58-encoded mint address of the quote token. Omit to receive every market. |
| `hydrate` | integer | No | Size of the row set to maintain, `1`–`100`. Sent as an initial snapshot, then held at this size (see [Row set behavior](#row-set-behavior)). |
| `throttle` | string | No | Minimum interval between pushes. A positive integer followed by `ms`, `s`, `m`, or `h` (for example `1s`). |

## Request Example

{% tabs %}
{% tab title="wscat" %}
{% code overflow="wrap" %}
```bash
# WebSocket-only. Connect first:
wscat -c 'wss://stream.eu-central-1.getblock.io/v1/solana-mainnet/stream?apiKey=<API-KEY>'

# Then send:
{"jsonrpc":"2.0","id":"getblock.io","method":"getblock_subscribe","params":[{"source":"market","topic":"block","params":{"base":"So11111111111111111111111111111111111111112","quote":"EPjFWdd5AufqSSqeM2qN1xzybapC8G4wEGGkZwyTDt1v","hydrate":5,"throttle":"1s"}}]}
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
        "topic": "block",
        "params": {
          "base": "So11111111111111111111111111111111111111112",
          "quote": "EPjFWdd5AufqSSqeM2qN1xzybapC8G4wEGGkZwyTDt1v",
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
              "topic": "block",
              "params": {
                "base": "So11111111111111111111111111111111111111112",
                "quote": "EPjFWdd5AufqSSqeM2qN1xzybapC8G4wEGGkZwyTDt1v",
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
          "base_volume": "4.562825066",
          "buy_swaps": "4",
          "buy_volume": "0.423198286",
          "buy_volume_usd": "43.8594931411658",
          "id": 7001225,
          "max": "103.79723552331",
          "max_usd": "103.79723552331",
          "min": "103.328258250718",
          "min_usd": "103.328258250718",
          "num_swaps": "7",
          "outlier_count": "0",
          "outlier_volume_pct": "0",
          "quote": "EPjFWdd5AufqSSqeM2qN1xzybapC8G4wEGGkZwyTDt1v",
          "quote_usd_price": "1",
          "quote_volume": "472.882681",
          "sell_swaps": "3",
          "sell_volume": "4.13962678",
          "sell_volume_usd": "429.023316895939",
          "slot": "445316612",
          "timestamp": "2026-09-08T10:33:32.000000000Z",
          "total_volume": "4.562825066",
          "volume_usd": "472.882681",
          "vwap": "103.638163461668",
          "vwap_raw": "103.638163461668",
          "vwap_usd": "103.638163461668",
          "vwap_usd_raw": "103.638163461668"
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
| `buy_swaps` | string | Number of buy swaps. |
| `buy_volume` | string | Volume attributed to buy swaps. |
| `buy_volume_usd` | string | Buy-swap volume expressed in USD. |
| `id` | number | Row identifier. Stable across `updates` and `deletes` — use it as the key for local state. |
| `max` | string | Highest observed price in the slot, in the quote token. |
| `max_usd` | string | Highest observed price in the slot, in USD. |
| `min` | string | Lowest observed price in the slot, in the quote token. |
| `min_usd` | string | Lowest observed price in the slot, in USD. |
| `num_swaps` | string | Total number of observed swaps. |
| `outlier_count` | string | Number of swaps excluded from the aggregate as outliers. |
| `outlier_volume_pct` | string | Share of volume excluded as outliers, in percent. |
| `quote` | string | Mint address of the quote token. |
| `quote_usd_price` | string | USD price of the quote token used for the USD conversions in this row. |
| `quote_volume` | string | Total volume denominated in the quote token. |
| `sell_swaps` | string | Number of sell swaps. |
| `sell_volume` | string | Volume attributed to sell swaps. |
| `sell_volume_usd` | string | Sell-swap volume expressed in USD. |
| `slot` | string | Solana slot covered by the aggregate. |
| `timestamp` | string | Timestamp of the latest activity included in the aggregate. |
| `total_volume` | string | Total trading volume for the slot. |
| `volume_usd` | string | Total trading volume expressed in USD. |
| `vwap` | string | Volume-weighted average price for the slot, in the quote token. |
| `vwap_raw` | string | Volume-weighted average price before outlier exclusion. Equal to `vwap` when `outlier_count` is `0`. |
| `vwap_usd` | string | Volume-weighted average price expressed in USD. |
| `vwap_usd_raw` | string | USD volume-weighted average price before outlier exclusion. |

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

* Slot-level market monitoring and microstructure analysis
* Per-slot buy/sell pressure without processing the full trade tape
* Detecting bursts of activity tied to a specific block
* Lightweight alternative to `trades` for high-volume pairs

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
