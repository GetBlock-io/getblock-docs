---
description: >-
  Stream normalized individual Solana trades for a token pair. Complete guide
  on how to use the trades topic in GetBlock Solana Market Data documentation.
---

# trades - Solana Market Data

The `trades` topic streams normalized individual trades for the selected pair, aggregated across supported Solana venues. Use it when your application needs transaction-level activity, or when you want to calculate your own prices, indicators, and aggregations rather than consuming a pre-computed metric.

{% hint style="warning" %}
**WebSocket-only topic.** Subscribe with `getblock_subscribe` over `wss://stream.eu-central-1.getblock.io/v1/solana-mainnet/stream`. The `limit`, `from`, and `to` query parameters are not supported by the streaming API.
{% endhint %}

## Parameters

Pass one structured request object to [`getblock_subscribe`](getblock_subscribe-market-data.md), with `"source": "market"` and `"topic": "trades"`. The fields below go inside the nested `params` object.

| Parameter | Type | Required | Description |
| --- | --- | --- | --- |
| `base` | string | No | Base58-encoded mint address of the base token. Omit to receive every market. |
| `quote` | string | No | Base58-encoded mint address of the quote token. Omit to receive every market. |
| `hydrate` | integer | No | Size of the row set to maintain, `1`–`100`. Sent as an initial snapshot, then held at this size (see [Row set behavior](#row-set-behavior)). |
| `throttle` | string | No | Minimum interval between pushes. A positive integer followed by `ms`, `s`, `m`, or `h` (for example `1s`). |

{% hint style="info" %}
On an active pair `trades` moves quickly, so most notifications carry an equal number of `inserts` and `deletes` as older trades leave the `hydrate` window. Size `hydrate` to the depth of tape you actually want to keep.
{% endhint %}

## Request Example

{% tabs %}
{% tab title="wscat" %}
{% code overflow="wrap" %}
```bash
# WebSocket-only. Connect first:
wscat -c 'wss://stream.eu-central-1.getblock.io/v1/solana-mainnet/stream?apiKey=<API-KEY>'

# Then send:
{"jsonrpc":"2.0","id":"getblock.io","method":"getblock_subscribe","params":[{"source":"market","topic":"trades","params":{"base":"So11111111111111111111111111111111111111112","quote":"EPjFWdd5AufqSSqeM2qN1xzybapC8G4wEGGkZwyTDt1v","hydrate":5,"throttle":"1s"}}]}
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
        "topic": "trades",
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
              "topic": "trades",
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
          "base_amount": "956805266",
          "base_decimals": "9",
          "base_volume": "0.956805266",
          "id": 46181421,
          "is_buy": false,
          "price": "103.609080679955",
          "quote": "EPjFWdd5AufqSSqeM2qN1xzybapC8G4wEGGkZwyTDt1v",
          "quote_amount": "99133714",
          "quote_decimals": "6",
          "quote_volume": "99.133714",
          "signature": "4mTtaVKjBGhoXxuaGBJeixXCprKhrMh6dxBTXhTue9nUhHw8GMxx6CjFFQKkZfw4nZVgv4EpyQYWjKPE5uAVzNHp",
          "signer": "FR8pQkTmRtaPr2LN7s9bzPK9fTxECM4DYf496Rqt8UaD",
          "slot": "445316600",
          "timestamp": "2026-09-08T10:33:28.000000000Z"
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
| `base_amount` | string | Base-token amount in the token's smallest units. |
| `base_decimals` | string | Decimal precision of the base token. |
| `base_volume` | string | Human-readable amount of the base token exchanged. |
| `id` | number | Row identifier. Stable across `updates` and `deletes` — use it as the key for local state. |
| `is_buy` | boolean | `true` when the observed trade is classified as a buy. |
| `price` | string | Trade price expressed in the quote token. |
| `quote` | string | Mint address of the quote token. |
| `quote_amount` | string | Quote-token amount in the token's smallest units. |
| `quote_decimals` | string | Decimal precision of the quote token. |
| `quote_volume` | string | Human-readable amount of the quote token exchanged. |
| `signature` | string | Solana transaction signature. |
| `signer` | string | Wallet that signed the transaction. |
| `slot` | string | Solana slot in which the trade was observed. |
| `timestamp` | string | Trade timestamp. |

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

* Feeding a custom indicator or aggregation pipeline that needs raw fills
* Trade tapes and "recent trades" panels in a trading interface
* Detecting individual large fills or wallet-level activity
* Reconstructing your own candles at a window the API does not offer

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
