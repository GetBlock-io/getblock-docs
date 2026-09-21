---
description: >-
  Stream raw EVM event logs filtered by contract address and topics. Complete guide on how to use the logs topic in GetBlock EVM Data Stream documentation.
---

# logs - EVM Data Stream

The `logs` topic sends **one notification per matching log** from newly mined receipts, in the same shape as `eth_getLogs`. Filtering works like `eth_subscribe("logs")`: by emitting contract and by positional topics.

{% hint style="warning" %}
**WebSocket-only topic.** Subscribe with [`getblock_subscribe`](getblock_subscribe-evm-data-stream.md) on any [network endpoint](./#endpoints).
{% endhint %}

## Parameters

<table data-search="false"><thead><tr><th>Parameter</th><th>Type</th><th>Required</th><th>Description</th></tr></thead><tbody>
<tr><td><code>filters.address</code></td><td>string or array</td><td>No</td><td>Emitting contract address, or an array of addresses to match any of.</td></tr>
<tr><td><code>filters.topics</code></td><td>array</td><td>No</td><td>Geth-style positional topic filter. Each position is an exact 32-byte hash, an array of hashes (OR), or <code>null</code> (wildcard).</td></tr>
</tbody></table>

How `filters.topics` matches, position by position:

| Filter                              | Matches                                                            |
| ----------------------------------- | ------------------------------------------------------------------ |
| `[]` or omitted                     | Any log                                                            |
| `["0xA"]`                           | `topic0` is `0xA`                                                  |
| `[null, "0xB"]`                     | Anything in `topic0`, and `topic1` is `0xB`                        |
| `[["0xA", "0xC"], null, "0xD"]`     | `topic0` is `0xA` **or** `0xC`, and `topic2` is `0xD`              |

Topic values are 32 bytes. To match an address in an indexed parameter, left-pad it with zeros: `0x000000000000000000000000` + the 20-byte address.

## Request Example

{% tabs %}
{% tab title="wscat" %}
{% code overflow="wrap" %}
```bash
# WebSocket-only. Connect first:
wscat -c 'wss://stream.eu-central-1.getblock.io/v1/eth-mainnet/stream' -H 'Authorization: Bearer <API-KEY>'

# Then send:
{"jsonrpc":"2.0","id":1,"method":"getblock_subscribe","params":[{"topic":"logs","filters":{"address":"0xdAC17F958D2ee523a2206206994597C13D831ec7","topics":["0xddf252ad1be2c89b69c2b068fc378daa952ba7f163c4a11628f55a4df523b3ef"]}}]}
```
{% endcode %}
{% endtab %}

{% tab title="JavaScript" %}
```javascript
// npm install ws
import WebSocket from 'ws';

const ws = new WebSocket('wss://stream.eu-central-1.getblock.io/v1/eth-mainnet/stream', {
  headers: { Authorization: 'Bearer <API-KEY>' }
});

ws.on('open', () => {
  ws.send(JSON.stringify({
    jsonrpc: '2.0',
    id: 1,
    method: 'getblock_subscribe',
    params: [{
      topic: 'logs',
      filters: {
        address: '0xdAC17F958D2ee523a2206206994597C13D831ec7',
        topics: [
          '0xddf252ad1be2c89b69c2b068fc378daa952ba7f163c4a11628f55a4df523b3ef'
        ]
      }
    }]
  }));
});

ws.on('message', (raw) => {
  const msg = JSON.parse(raw);

  // First reply is the subscription ID (or an error)
  if (msg.id !== undefined) {
    console.log(msg.error ?? `Subscribed: ${msg.result}`);
    return;
  }

  const { removed, data } = msg.params.result;
  console.log(removed ? 'REMOVED' : 'log', data.address, data.topics[0], data.transactionHash);
});
```
{% endtab %}

{% tab title="Python" %}
```python
# pip install "websockets>=14"
import asyncio, json, websockets

URL = "wss://stream.eu-central-1.getblock.io/v1/eth-mainnet/stream"
HEADERS = {"Authorization": "Bearer <API-KEY>"}

async def main():
    # ping_interval=None: the server sends its own keepalive pings (see Connection keepalive)
    async with websockets.connect(URL, additional_headers=HEADERS, ping_interval=None) as ws:
        await ws.send(json.dumps({
            "jsonrpc": "2.0",
            "id": 1,
            "method": "getblock_subscribe",
            "params": [{
                "topic": "logs",
                "filters": {
                    "address": "0xdAC17F958D2ee523a2206206994597C13D831ec7",
                    "topics": [
                        "0xddf252ad1be2c89b69c2b068fc378daa952ba7f163c4a11628f55a4df523b3ef"
                    ]
                }
            }]
        }))

        async for message in ws:
            msg = json.loads(message)

            # First reply is the subscription ID (or an error)
            if "id" in msg:
                print(msg.get("error") or f"Subscribed: {msg['result']}")
                continue

            event = msg["params"]["result"]
            removed, data = event["removed"], event["data"]
            print('REMOVED' if removed else 'log', data['address'], data['topics'][0], data['transactionHash'])

asyncio.run(main())
```
{% endtab %}
{% endtabs %}

## Response Example

The first reply is the subscription ID:

```json
{
  "jsonrpc": "2.0",
  "id": 1,
  "result": "0x69f577474261234f921cb6373af9a9f4"
}
```

Every later message is an event notification:

```json
{
  "jsonrpc": "2.0",
  "method": "getblock_subscribe",
  "params": {
    "subscription": "0x69f577474261234f921cb6373af9a9f4",
    "result": {
      "eventId": "eth:mainnet:receipts:0x19ddc6864bd5ec1cdbd493e3d3653b529d79293a677…",
      "removed": false,
      "revision": 116876401,
      "data": {
        "address": "0xdac17f958d2ee523a2206206994597c13d831ec7",
        "topics": [
          "0xddf252ad1be2c89b69c2b068fc378daa952ba7f163c4a11628f55a4df523b3ef",
          "0x0000000000000000000000004b3dd3690ba06b2a73089e02381d0eba9a7dbca5",
          "0x000000000000000000000000a2050435e59f41300b7787808e976255faa198e5"
        ],
        "data": "0x0000000000000000000000000000000000000000000000000000000006f4b360",
        "blockNumber": "0x18d1e1e",
        "transactionHash": "0x6b362543c83c1d18b27701235338c05a7e04c2e5098bed46d60afe9a0f17d2a1",
        "transactionIndex": "0x3",
        "blockHash": "0x19ddc6864bd5ec1cdbd493e3d3653b529d79293a6773517018153cdff6ac0be2",
        "blockTimestamp": "0x6ab110c3",
        "logIndex": "0x3",
        "removed": false
      }
    }
  }
}
```

## Response Parameters

The `result` object contains the event envelope and the payload in `data`.

<table data-search="false"><thead><tr><th>Field</th><th>Type</th><th>Description</th></tr></thead><tbody>
<tr><td><code>eventId</code></td><td>string</td><td>Stable event identifier. A reorg correction reuses it.</td></tr>
<tr><td><code>removed</code></td><td>boolean</td><td><code>true</code> when a reorg removed this previously delivered event.</td></tr>
<tr><td><code>revision</code></td><td>number</td><td>Event version. For one <code>eventId</code>, a greater revision is newer.</td></tr>
<tr><td><code>data</code></td><td>object</td><td>The log, in the same shape as an <code>eth_getLogs</code> entry.</td></tr>
<tr><td><code>data.topics</code></td><td>array</td><td>Indexed topics. <code>topics[0]</code> is the event signature hash, except for anonymous events.</td></tr>
<tr><td><code>data.data</code></td><td>string</td><td>ABI-encoded non-indexed event arguments.</td></tr>
<tr><td><code>data.address</code></td><td>string</td><td>Address of the contract that emitted the log.</td></tr>
<tr><td><code>data.blockNumber</code></td><td>string</td><td>Block number, hex-encoded.</td></tr>
<tr><td><code>data.blockHash</code></td><td>string</td><td>Hash of the block that contains the log.</td></tr>
<tr><td><code>data.blockTimestamp</code></td><td>string</td><td>Block timestamp in Unix seconds, hex-encoded.</td></tr>
<tr><td><code>data.transactionHash</code></td><td>string</td><td>Hash of the transaction that emitted the log.</td></tr>
<tr><td><code>data.transactionIndex</code></td><td>string</td><td>Transaction's position in the block, hex-encoded.</td></tr>
<tr><td><code>data.logIndex</code></td><td>string</td><td>Log's position in the block, hex-encoded.</td></tr>
<tr><td><code>data.removed</code></td><td>boolean</td><td>The underlying log's <code>removed</code> flag. Use the envelope's <code>removed</code> for reorg handling.</td></tr>
</tbody></table>

To get named, decoded arguments instead of raw `topics` and `data`, use [`decodedLogs`](decodedlogs-evm-data-stream.md).

## Use Cases

* Following any contract event, such as swaps, deposits, liquidations, or governance votes
* Porting an existing `eth_subscribe("logs")` or `eth_getLogs` polling integration
* Decoding events yourself with a full ABI and your own library
* Watching a known event across many contracts by filtering on `topic0` only

## Error Handling

<table data-search="false"><thead><tr><th>Code</th><th>Message</th><th>Cause</th></tr></thead><tbody>
<tr><td><code>-32602</code></td><td><code>invalid logs filter: invalid address "…"</code></td><td><code>filters.address</code> is not a 20-byte hex address.</td></tr>
<tr><td><code>-32602</code></td><td><code>invalid logs request: json: unknown field "…"</code></td><td>The request has a field this topic does not accept.</td></tr>
<tr><td><code>-32001</code></td><td><code>subscription limit exceeded</code></td><td>You already have 15 active or pending subscriptions.</td></tr>
</tbody></table>
