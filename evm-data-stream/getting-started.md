---
description: >-
  Connect to GetBlock EVM Data Stream, open your first subscription, and handle
  live events in Node.js or Python.
---

# Getting Started

This guide takes you from an API key to a running client that receives live ERC-20 transfers on Ethereum.

### 1. Get an API key

1. Log in to your [GetBlock account](https://account.getblock.io/).
2. Open **Settings → API Keys** and copy a key. Keys start with `gb_`.

Data Stream is billed from your CU balance at **10 CU per delivered event**, so make sure your account has CU available.

### 2. Choose a network

Each network has its own endpoint:

| Network           | WebSocket endpoint                                                  |
| ----------------- | ------------------------------------------------------------------- |
| Ethereum Mainnet  | `wss://stream.eu-central-1.getblock.io/v1/eth-mainnet/stream`       |
| BNB Smart Chain   | `wss://stream.eu-central-1.getblock.io/v1/bsc-mainnet/stream`       |
| Polygon Mainnet   | `wss://stream.eu-central-1.getblock.io/v1/polygon-mainnet/stream`   |
| Robinhood Mainnet | `wss://stream.eu-central-1.getblock.io/v1/robinhood-mainnet/stream` |

To use more than one network, open one connection per network.

### 3. Choose a topic and filters

A **topic** decides what kind of event you receive; **filters** narrow it down. For example:

| Goal                                    | Topic                 | Filters                     |
| --------------------------------------- | --------------------- | --------------------------- |
| New block headers                       | `newHeads`            | none                        |
| USDT transfers into one wallet          | `erc20Transfers`      | `tokenContract`, `to`       |
| Swap events from one pool, decoded      | `decodedLogs`         | `abi`, `address`            |
| Failed transactions sent by your wallet | `transactionReceipts` | `status: "failure"`, `from` |

See [API Reference → Topics](api-reference/#topics) for all 15 topics.

{% hint style="info" %}
You pay for every event delivered after filtering. A subscription with no filters on a busy topic such as `erc20Transfers` delivers every transfer on the network. Start narrow.
{% endhint %}

### 4. Connect and subscribe

Authenticate with an `Authorization: Bearer` header, then send `getblock_subscribe` with one request object.

{% tabs %}
{% tab title="wscat" %}
{% code overflow="wrap" %}
```bash
npm install -g wscat
wscat -c 'wss://stream.eu-central-1.getblock.io/v1/eth-mainnet/stream' -H 'Authorization: Bearer <API-KEY>'

# Once connected, send:
{"jsonrpc":"2.0","id":1,"method":"getblock_subscribe","params":[{"topic":"erc20Transfers","filters":{"tokenContract":"0xdAC17F958D2ee523a2206206994597C13D831ec7"}}]}
```
{% endcode %}
{% endtab %}

{% tab title="Node.js" %}
{% code overflow="wrap" %}
```javascript
// npm install ws
import WebSocket from 'ws';

const USDT = '0xdAC17F958D2ee523a2206206994597C13D831ec7';

const ws = new WebSocket('wss://stream.eu-central-1.getblock.io/v1/eth-mainnet/stream', {
  headers: { Authorization: `Bearer ${process.env.GETBLOCK_API_KEY}` }
});

ws.on('open', () => {
  ws.send(JSON.stringify({
    jsonrpc: '2.0',
    id: 1,
    method: 'getblock_subscribe',
    params: [{ topic: 'erc20Transfers', filters: { tokenContract: USDT } }]
  }));
});

ws.on('message', (raw) => {
  const msg = JSON.parse(raw);

  // Replies to your requests carry the request id
  if (msg.id !== undefined) {
    if (msg.error) throw new Error(`${msg.error.code}: ${msg.error.message}`);
    console.log('Subscription ID:', msg.result);
    return;
  }

  // Everything else is an event
  const { removed, data } = msg.params.result;
  const amount = Number(BigInt(data.value)) / 1e6; // USDT has 6 decimals
  console.log(`${removed ? 'REVERTED' : 'transfer'} ${amount} USDT  ${data.from} → ${data.to}`);
});

ws.on('close', (code) => console.log('closed', code));
```
{% endcode %}
{% endtab %}

{% tab title="Python" %}
{% code overflow="wrap" %}
```python
# pip install "websockets>=14"
import asyncio, json, os, websockets

URL = "wss://stream.eu-central-1.getblock.io/v1/eth-mainnet/stream"
USDT = "0xdAC17F958D2ee523a2206206994597C13D831ec7"

async def main():
    headers = {"Authorization": f"Bearer {os.environ['GETBLOCK_API_KEY']}"}
    # ping_interval=None: the server sends its own keepalive pings
    async with websockets.connect(URL, additional_headers=headers, ping_interval=None) as ws:
        await ws.send(json.dumps({
            "jsonrpc": "2.0",
            "id": 1,
            "method": "getblock_subscribe",
            "params": [{"topic": "erc20Transfers", "filters": {"tokenContract": USDT}}]
        }))

        async for message in ws:
            msg = json.loads(message)

            if "id" in msg:
                if "error" in msg:
                    raise RuntimeError(msg["error"])
                print("Subscription ID:", msg["result"])
                continue

            event = msg["params"]["result"]
            data = event["data"]
            amount = int(data["value"], 16) / 1e6  # USDT has 6 decimals
            label = "REVERTED" if event["removed"] else "transfer"
            print(f"{label} {amount} USDT  {data['from']} → {data['to']}")

asyncio.run(main())
```
{% endcode %}
{% endtab %}
{% endtabs %}

{% hint style="info" %}
Connecting from a browser? Browser `WebSocket` APIs cannot set headers, so append the key instead: `…/eth-mainnet/stream?apiKey=<API-KEY>`. Keep query-string keys out of logs.
{% endhint %}

### 5. Read the response

The first message is the subscription ID:

```json
{ "jsonrpc": "2.0", "id": 1, "result": "0x573e3382e8ea7d534f267b232aae22dd" }
```

Each event then arrives as a notification. The payload sits in `params.result.data`:

```json
{
  "jsonrpc": "2.0",
  "method": "getblock_subscribe",
  "params": {
    "subscription": "0x573e3382e8ea7d534f267b232aae22dd",
    "result": {
      "eventId": "eth:mainnet:receipts:0x19dd…0be2:erc20Transfers:0x19dd…0be2:0x6b36…d2a1:0x3",
      "removed": false,
      "revision": 116876401,
      "data": {
        "tokenContract": "0xdac17f958d2ee523a2206206994597c13d831ec7",
        "from": "0x4b3dd3690ba06b2a73089e02381d0eba9a7dbca5",
        "to": "0xa2050435e59f41300b7787808e976255faa198e5",
        "value": "0x6f4b360",
        "blockNumber": "0x18d1e1e",
        "transactionHash": "0x6b362543c83c1d18b27701235338c05a7e04c2e5098bed46d60afe9a0f17d2a1",
        "logIndex": "0x3",
        "…": "…"
      }
    }
  }
}
```

`eventId`, `removed`, and `revision` are how the stream reports [chain reorganizations](handling-chain-reorganizations.md). If you store events, key them by `eventId`.

### 6. Unsubscribe or disconnect

Cancel one subscription and keep the socket open:

{% code overflow="wrap" %}
```json
{ 
    "jsonrpc": "2.0", 
    "id": 2, 
    "method": "getblock_unsubscribe", 
    "params": [{ "subscription": "0x573e3382e8ea7d534f267b232aae22dd" }] 
}
```
{% endcode %}

Closing the socket ends all its subscriptions.

### Production checklist

* [ ] **Let the server drive keepalive:** The server pings every 30 seconds. Disable client-side pings in Python `websockets` (`ping_interval=None`); see [Connection keepalive](api-reference/#connection-keepalive).
* [ ] **Reconnect with backoff.** The stream does not replay events missed while disconnected. After reconnecting, resubscribe and backfill the gap from an RPC node if needed.
* [ ] **Handle `removed: true`.** Reorg corrections arrive on the same subscription. See [Handling Chain Reorganizations](handling-chain-reorganizations.md).
* [ ] **Deduplicate by `eventId` and `revision`.** Delivery is at-least-once.
* [ ] **Stay within limits:** 15 subscriptions and 50 connections per user, across all networks.
* [ ] **Ignore unknown fields** so new payload fields don't break your parser.
