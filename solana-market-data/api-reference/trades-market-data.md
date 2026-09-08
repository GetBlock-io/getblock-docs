---
description: >-
  Stream normalized individual Solana trades for a token pair. Complete guide on
  how to use the trades topic in GetBlock Solana Market Data documentation.
---

# trades - Solana Market Data

The `trades` topic streams normalized individual trades for the selected pair, aggregated across supported Solana venues. Use it when your application needs transaction-level activity, or when you want to calculate your own prices, indicators, and aggregations rather than consuming a pre-computed metric.

{% hint style="warning" %}
**WebSocket-only topic.** Subscribe with `getblock_subscribe` over `wss://stream.eu-central-1.getblock.io/v1/solana-mainnet/stream`. The `limit`, `from`, and `to` query parameters are not supported by the streaming API.
{% endhint %}

## Parameters

Pass one structured request object to [`getblock_subscribe`](getblock_subscribe-market-data.md), with `"source": "market"` and `"topic": "trades"`. The fields below go inside the nested `params` object.

| Parameter  | Type    | Required | Description                                                                                                                                                       |
| ---------- | ------- | -------- | ----------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| `base`     | string  | No       | Base58-encoded mint address of the base token. Omit to receive every market.                                                                                      |
| `quote`    | string  | No       | Base58-encoded mint address of the quote token. Omit to receive every market.                                                                                     |
| `hydrate`  | integer | No       | Size of the row set to maintain, `1`–`100`. Sent as an initial snapshot, then held at this size (see [Row set behavior](trades-market-data.md#row-set-behavior)). |
| `throttle` | string  | No       | Minimum interval between pushes. A positive integer followed by `ms`, `s`, `m`, or `h` (for example `1s`).                                                        |

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

<table data-search="false"><thead><tr><th>Field</th><th>Type</th><th>Description</th></tr></thead><tbody><tr><td><code>base</code></td><td>string</td><td>Mint address of the base token.</td></tr><tr><td><code>base_amount</code></td><td>string</td><td>Base-token amount in the token's smallest units.</td></tr><tr><td><code>base_decimals</code></td><td>string</td><td>Decimal precision of the base token.</td></tr><tr><td><code>base_volume</code></td><td>string</td><td>Human-readable amount of the base token exchanged.</td></tr><tr><td><code>id</code></td><td>number</td><td>Row identifier. Stable across <code>updates</code> and <code>deletes</code> — use it as the key for local state.</td></tr><tr><td><code>is_buy</code></td><td>boolean</td><td><code>true</code> when the observed trade is classified as a buy.</td></tr><tr><td><code>price</code></td><td>string</td><td>Trade price expressed in the quote token.</td></tr><tr><td><code>quote</code></td><td>string</td><td>Mint address of the quote token.</td></tr><tr><td><code>quote_amount</code></td><td>string</td><td>Quote-token amount in the token's smallest units.</td></tr><tr><td><code>quote_decimals</code></td><td>string</td><td>Decimal precision of the quote token.</td></tr><tr><td><code>quote_volume</code></td><td>string</td><td>Human-readable amount of the quote token exchanged.</td></tr><tr><td><code>signature</code></td><td>string</td><td>Solana transaction signature.</td></tr><tr><td><code>signer</code></td><td>string</td><td>Wallet that signed the transaction.</td></tr><tr><td><code>slot</code></td><td>string</td><td>Solana slot in which the trade was observed.</td></tr><tr><td><code>timestamp</code></td><td>string</td><td>Trade timestamp.</td></tr></tbody></table>

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

<table data-search="false"><thead><tr><th>Code</th><th>Message</th><th>Cause</th></tr></thead><tbody><tr><td><code>-32602</code></td><td><code>unsupported source "…": supported sources are "market" and "priorityfee"</code></td><td><code>source</code> is not a recognized value.</td></tr><tr><td><code>-32602</code></td><td><code>unsupported topic: supported topics are trades, ohlcv, block, twap, vwap, volume, token</code></td><td><code>topic</code> is not a recognized value.</td></tr><tr><td><code>-32602</code></td><td><code>window must be one of 1s, 10s, 30s, 1m, …</code></td><td><code>window</code> is missing or not an accepted value on a windowed topic.</td></tr><tr><td><code>-32602</code></td><td><code>hydrate must be an integer from 1 to 100</code></td><td><code>hydrate</code> is outside the accepted range.</td></tr><tr><td><code>-32602</code></td><td><code>throttle must be a positive integer followed by ms, s, m, or h</code></td><td><code>throttle</code> is malformed.</td></tr><tr><td><code>-32602</code></td><td><code>base must be a base58-encoded 32-byte Solana public key</code></td><td><code>base</code> or <code>quote</code> is not a valid mint address.</td></tr><tr><td>HTTP <code>401</code></td><td><code>authorization failed</code></td><td>API key missing, invalid, or Solana Market Data is not activated on it. WebSocket clients cannot read the handshake body and surface this as close code <code>1006</code>.</td></tr></tbody></table>
