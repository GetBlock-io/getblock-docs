---
description: >-
  Stream Solana token metadata by mint address. Complete guide on how to use the
  token topic in GetBlock Solana Market Data documentation.
hidden: true
---

# token - Solana Market Data

The `token` topic streams metadata for a single token selected with the `mint` parameter — name, ticker, decimal precision, and current supply. It does not use `base`, `quote`, or `window`.

{% hint style="danger" %}
**This topic is currently not returning data.** The service accepts `token` as a valid topic name, but a subscription to it receives no acknowledgement, no data, and no error — the request is silently dropped and the client waits indefinitely. Verified against `mint` values for SOL and USDC, with `mint` omitted, and with empty `params`.

Do not build against this topic yet, and set a client-side timeout on the subscription acknowledgement so a request cannot hang. For token decimals in the meantime, use `base_decimals` and `quote_decimals` from the [`trades`](trades-market-data.md) topic.
{% endhint %}

## Parameters

Pass one structured request object to [`getblock_subscribe`](getblock_subscribe-market-data.md), with `"source": "market"` and `"topic": "token"`. The fields below go inside the nested `params` object.

| Parameter  | Type    | Required | Description                                                                             |
| ---------- | ------- | -------- | --------------------------------------------------------------------------------------- |
| `mint`     | string  | Yes      | Base58-encoded mint address of the token to track.                                      |
| `hydrate`  | integer | No       | Size of the row set to maintain, `1`–`100`.                                             |
| `throttle` | string  | No       | Minimum interval between pushes. A positive integer followed by `ms`, `s`, `m`, or `h`. |

`base`, `quote`, and `window` are not used by this topic.

## Request Example

{% tabs %}
{% tab title="wscat" %}
{% code overflow="wrap" %}
```bash
# WebSocket-only. Connect first:
wscat -c 'wss://stream.eu-central-1.getblock.io/v1/solana-mainnet/stream?apiKey=<API-KEY>'

# Then send:
{"jsonrpc":"2.0","id":"getblock.io","method":"getblock_subscribe","params":[{"source":"market","topic":"token","params":{"mint":"So11111111111111111111111111111111111111112","hydrate":5}}]}
```
{% endcode %}
{% endtab %}

{% tab title="JavaScript" %}
```javascript
const ws = new WebSocket(
  'wss://stream.eu-central-1.getblock.io/v1/solana-mainnet/stream?apiKey=<API-KEY>'
);

ws.onopen = () => {
  ws.send(JSON.stringify({
    jsonrpc: '2.0',
    id: 'getblock.io',
    method: 'getblock_subscribe',
    params: [{
      source: 'market',
      topic: 'token',
      params: { mint: 'So11111111111111111111111111111111111111112', hydrate: 5 }
    }]
  }));

  // The topic currently does not acknowledge. Fail fast rather than hang.
  setTimeout(() => {
    if (!subscriptionId) {
      console.error('No subscription acknowledgement — token topic unavailable.');
      ws.close();
    }
  }, 10_000);
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
                "topic": "token",
                "params": {"mint": "So11111111111111111111111111111111111111112", "hydrate": 5}
            }]
        }))

        # The topic currently does not acknowledge. Fail fast rather than hang.
        try:
            message = await asyncio.wait_for(ws.recv(), timeout=10)
        except asyncio.TimeoutError:
            raise RuntimeError('No subscription acknowledgement — token topic unavailable.')

asyncio.run(main())
```
{% endtab %}
{% endtabs %}

## Response Example

{% hint style="info" %}
The shape below is the specified response for this topic. It could not be captured from a live subscription because the topic does not currently return data, so treat it as provisional until the topic is working.
{% endhint %}

```json
{
  "jsonrpc": "2.0",
  "method": "getblock_subscribe",
  "params": {
    "subscription": "0x4503d281ca474f3322ed277fde75db15",
    "result": {
      "inserts": [
        {
          "id": 1,
          "mint": "So11111111111111111111111111111111111111112",
          "name": "Wrapped SOL",
          "symbol": "SOL",
          "decimals": "9",
          "supply": "0"
        }
      ],
      "updates": [],
      "deletes": []
    }
  }
}
```

## Response Parameters

| Field      | Type   | Description                                            |
| ---------- | ------ | ------------------------------------------------------ |
| `id`       | number | Row identifier. Stable across `updates` and `deletes`. |
| `mint`     | string | Token mint address.                                    |
| `name`     | string | Token name.                                            |
| `symbol`   | string | Token ticker symbol.                                   |
| `decimals` | string | Decimal precision of the token.                        |
| `supply`   | string | Current token supply.                                  |

## Use Cases

* Resolving a mint address to a display name and ticker in a UI
* Reading decimal precision to convert raw amounts to human-readable values
* Tracking supply changes for a token
* Enriching trade and candle rows, which carry mint addresses rather than symbols

## Error Handling

| Code       | Message                                                   | Cause                                                                       |
| ---------- | --------------------------------------------------------- | --------------------------------------------------------------------------- |
| _(none)_   | _No response_                                             | Known issue — the topic accepts the subscription but never acknowledges it. |
| `-32602`   | `base must be a base58-encoded 32-byte Solana public key` | `mint` is not a valid mint address.                                         |
| `-32602`   | `hydrate must be an integer from 1 to 100`                | `hydrate` is outside the accepted range.                                    |
| HTTP `401` | `authorization failed`                                    | API key missing, invalid, or the product is not activated on it.            |
