---
description: >-
  Open a Solana Market Data subscription over WebSocket. Complete guide on how to
  use getblock_subscribe in GetBlock Solana Market Data documentation.
---

# getblock\_subscribe - Solana Market Data

`getblock_subscribe` opens a streaming subscription. It is the only method used to start a stream — the topic you pass decides which data model you receive. The service replies with a subscription ID, optionally sends an initial snapshot, and then pushes change sets until you unsubscribe or the socket closes.

{% hint style="warning" %}
**WebSocket-only method.** It will not work over HTTP POST.
{% endhint %}

## Endpoint

{% code overflow="wrap" %}
```
wss://stream.eu-central-1.getblock.io/v1/solana-mainnet/stream?apiKey=<API-KEY>
```
{% endcode %}

The API key is passed as a query parameter, not as a header. Create one in [Dashboard → API Keys](https://account.getblock.io/products/solana-data-stream#api-keys) and make sure Solana Market Data is activated on it.

## Parameters

`params` is an array containing exactly **one** request object:

| Parameter | Type   | Required | Description                                                                                              |
| --------- | ------ | -------- | -------------------------------------------------------------------------------------------------------- |
| `source`  | string | Yes      | Data source. `market` for Solana Market Data, or `priorityfee` for priority-fee data.                    |
| `topic`   | string | Yes      | Data model to receive. One of `trades`, `block`, `ohlcv`, `twap`, `vwap`, `volume`, `token`.              |
| `params`  | object | Yes      | Topic-specific parameters — the market pair, window, and delivery options. See the table below.          |

### Market parameters

These go inside the nested `params` object.

| Field      | Type    | Required                | Description                                                                                                        |
| ---------- | ------- | ----------------------- | -------------------------------------------------------------------------------------------------------------------- |
| `base`     | string  | No                      | Base58-encoded mint address of the base token.                                                                     |
| `quote`    | string  | No                      | Base58-encoded mint address of the quote token.                                                                    |
| `mint`     | string  | Yes for `token`         | Base58-encoded token mint, used only by the `token` topic.                                                         |
| `window`   | string  | Yes for windowed topics | Aggregation period. Required by `ohlcv`, `twap`, `vwap`, and `volume`.                                             |
| `hydrate`  | integer | No                      | Size of the row set to maintain, `1`–`100`. Sent as an initial snapshot, then held at this size.                   |
| `throttle` | string  | No                      | Minimum interval between pushes. A positive integer followed by `ms`, `s`, `m`, or `h`.                            |

Accepted `window` values for `market` topics:

```
1s, 10s, 30s, 1m, 5m, 30m, 1h, 2h, 4h, 6h, 8h, 12h, 24h
```

{% hint style="info" %}
`base` and `quote` are optional. **Omitting both subscribes you to every market**, not to a default pair — a high-volume firehose spanning hundreds of pairs. Set both unless you genuinely want all markets.
{% endhint %}

`limit`, `from`, and `to` are query-only parameters and are not supported by the streaming API.

## Request Example

{% tabs %}
{% tab title="wscat" %}
{% code overflow="wrap" %}
```bash
# WebSocket-only. Connect first:
wscat -c 'wss://stream.eu-central-1.getblock.io/v1/solana-mainnet/stream?apiKey=<API-KEY>'

# Then send:
{"jsonrpc":"2.0","id":"getblock.io","method":"getblock_subscribe","params":[{"source":"market","topic":"ohlcv","params":{"base":"So11111111111111111111111111111111111111112","quote":"EPjFWdd5AufqSSqeM2qN1xzybapC8G4wEGGkZwyTDt1v","window":"1m","hydrate":10,"throttle":"1s"}}]}
```
{% endcode %}
{% endtab %}

{% tab title="JavaScript" %}
```javascript
// Node 20+ has a global WebSocket; no dependency required.
const ws = new WebSocket(
  'wss://stream.eu-central-1.getblock.io/v1/solana-mainnet/stream?apiKey=<API-KEY>'
);

const rows = new Map();
let subscriptionId = null;

ws.onopen = () => {
  ws.send(JSON.stringify({
    jsonrpc: '2.0',
    id: 'getblock.io',
    method: 'getblock_subscribe',
    params: [{
      source: 'market',
      topic: 'ohlcv',
      params: {
        base: 'So11111111111111111111111111111111111111112',
        quote: 'EPjFWdd5AufqSSqeM2qN1xzybapC8G4wEGGkZwyTDt1v',
        window: '1m',
        hydrate: 10,
        throttle: '1s'
      }
    }]
  }));
};

ws.onmessage = (event) => {
  const msg = JSON.parse(event.data);

  if (typeof msg.result === 'string') {
    subscriptionId = msg.result;
    console.log('Subscribed:', subscriptionId);
    return;
  }

  const { inserts = [], updates = [], deletes = [] } = msg.params.result;
  for (const row of [...inserts, ...updates]) rows.set(row.id, row);
  for (const row of deletes) rows.delete(row.id);
};
```
{% endtab %}

{% tab title="Python" %}
```python
import asyncio, json, websockets

URL = 'wss://stream.eu-central-1.getblock.io/v1/solana-mainnet/stream?apiKey=<API-KEY>'

async def main():
    async with websockets.connect(URL) as ws:
        await ws.send(json.dumps({
            "jsonrpc": "2.0",
            "id": "getblock.io",
            "method": "getblock_subscribe",
            "params": [{
                "source": "market",
                "topic": "ohlcv",
                "params": {
                    "base": "So11111111111111111111111111111111111111112",
                    "quote": "EPjFWdd5AufqSSqeM2qN1xzybapC8G4wEGGkZwyTDt1v",
                    "window": "1m",
                    "hydrate": 10,
                    "throttle": "1s"
                }
            }]
        }))

        rows = {}
        async for message in ws:
            msg = json.loads(message)

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

The first reply confirms the subscription and returns its ID:

```json
{
  "jsonrpc": "2.0",
  "id": "getblock.io",
  "result": "0x4503d281ca474f3322ed277fde75db15"
}
```

Every message after that is a change-set notification. Note that the payload is nested under `params.result`, not `result`:

```json
{
  "jsonrpc": "2.0",
  "method": "getblock_subscribe",
  "params": {
    "subscription": "0x4503d281ca474f3322ed277fde75db15",
    "result": {
      "inserts": [ { "id": 856191, "…": "…" } ],
      "updates": [],
      "deletes": []
    }
  }
}
```

## Response Parameters

| Field                      | Type   | Description                                                                        |
| -------------------------- | ------ | ------------------------------------------------------------------------------------ |
| `result`                   | string | Hex-encoded subscription ID, returned once in reply to the subscribe request.      |
| `params.subscription`      | string | The subscription ID a notification belongs to. Use it to demultiplex the socket.   |
| `params.result.inserts`    | array  | New rows, including the initial `hydrate` snapshot.                                |
| `params.result.updates`    | array  | Full replacement rows for previously emitted IDs whose values changed.             |
| `params.result.deletes`    | array  | Rows to remove from local state by `id`. Carries the whole row, not just the ID.   |

A change-set property can be omitted when a notification contains no changes of that type. Process all three categories and ignore unknown row fields for forward compatibility.

## Multiple subscriptions

One socket can carry several subscriptions at once. Send a `getblock_subscribe` request per stream, keep the returned IDs, and route incoming notifications on `params.subscription`:

```javascript
const handlers = new Map();

function subscribe(id, request, handler) {
  pending.set(id, handler);
  ws.send(JSON.stringify({ jsonrpc: '2.0', id, method: 'getblock_subscribe', params: [request] }));
}

// in onmessage:
if (typeof msg.result === 'string') {
  handlers.set(msg.result, pending.get(msg.id));   // subscription ID → handler
} else {
  handlers.get(msg.params.subscription)?.(msg.params.result);
}
```

## Optimistic delivery and reconciliation

Solana produces an ordered sequence of slots, but recently observed data can still change before the network reaches finality. Waiting for finalization before publishing every market event would add latency and make the feed less useful for trading, monitoring, and automated execution.

Market Data therefore uses an optimistic streaming model. Rows are delivered as soon as they are available and can be corrected later through the same subscription.

The `window` parameter defines the aggregation period, not how long you wait for a first result. A subscription with `"window": "10s"` can receive a row before the ten-second window closes; as more trades enter that window, the service sends the latest version of the row in `updates`.

{% hint style="warning" %}
Do not treat an `insert` as final merely because it was delivered first, and do not treat a `delete` as a reorg signal — on fast-moving topics most deletes are ordinary eviction from the `hydrate` window. Applications that require finalized-only data should apply their own confirmation policy.
{% endhint %}

## Error Handling

| Code        | Message                                                                              | Cause                                                                                        |
| ----------- | ------------------------------------------------------------------------------------ | ---------------------------------------------------------------------------------------------- |
| `-32602`    | `unsupported source "…": supported sources are "market" and "priorityfee"`           | `source` is not a recognized value.                                                          |
| `-32602`    | `unsupported topic: supported topics are trades, ohlcv, block, twap, vwap, volume, token` | `topic` is not a recognized value.                                                       |
| `-32602`    | `window must be one of 1s, 10s, 30s, 1m, …`                                          | `window` is missing or not accepted on a windowed topic.                                     |
| `-32602`    | `hydrate must be an integer from 1 to 100`                                           | `hydrate` is outside the accepted range.                                                     |
| `-32602`    | `throttle must be a positive integer followed by ms, s, m, or h`                     | `throttle` is malformed.                                                                     |
| `-32602`    | `base must be a base58-encoded 32-byte Solana public key`                            | `base` or `quote` is not a valid mint address.                                               |
| HTTP `401`  | `authorization failed`                                                               | API key missing, invalid, or the product is not activated on it.                             |

{% hint style="info" %}
**Socket closes immediately with code `1006` and no reason.** The server rejects unauthorized connections with HTTP `401` during the WebSocket handshake, but browser and Node `WebSocket` clients cannot read a handshake response body — they surface only `1006`. Check that the `apiKey` query parameter is present and correct, and that Solana Market Data is activated on that key.
{% endhint %}
