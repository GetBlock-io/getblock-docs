---
description: >-
  Stream decoded ERC-20 Transfer events filtered by token, sender, and recipient. Complete guide on how to use the erc20Transfers topic in GetBlock EVM Data Stream documentation.
---

# erc20Transfers - EVM Data Stream

The `erc20Transfers` topic sends one notification per ERC-20 `Transfer` event, **already decoded** into token contract, sender, recipient, and raw amount. Use it for payment detection, wallet activity, and token-flow monitoring.

{% hint style="warning" %}
**WebSocket-only topic.** Subscribe with [`getblock_subscribe`](getblock_subscribe-evm-data-stream.md) on any [network endpoint](./#endpoints).
{% endhint %}

## Parameters

<table data-search="false"><thead><tr><th>Parameter</th><th>Type</th><th>Required</th><th>Description</th></tr></thead><tbody>
<tr><td><code>filters.tokenContract</code></td><td>string</td><td>No</td><td>Token contract address. Omit to receive transfers of every ERC-20 token.</td></tr>
<tr><td><code>filters.from</code></td><td>string</td><td>No</td><td>Sender address.</td></tr>
<tr><td><code>filters.to</code></td><td>string</td><td>No</td><td>Recipient address.</td></tr>
</tbody></table>

Filters combine with AND: `tokenContract` + `to` means "this token, sent to this address". To watch a wallet's incoming *and* outgoing transfers, open two subscriptions, one with `from` and one with `to`.

## Request Example

{% tabs %}
{% tab title="wscat" %}
{% code overflow="wrap" %}
```bash
# WebSocket-only. Connect first:
wscat -c 'wss://stream.eu-central-1.getblock.io/v1/eth-mainnet/stream' -H 'Authorization: Bearer <API-KEY>'

# Then send:
{"jsonrpc":"2.0","id":1,"method":"getblock_subscribe","params":[{"topic":"erc20Transfers","filters":{"tokenContract":"0xdAC17F958D2ee523a2206206994597C13D831ec7","to":"0xa2050435e59f41300b7787808e976255faa198e5"}}]}
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
      topic: 'erc20Transfers',
      filters: {
        tokenContract: '0xdAC17F958D2ee523a2206206994597C13D831ec7',
        to: '0xa2050435e59f41300b7787808e976255faa198e5'
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
  // value is in the token's smallest unit; USDT has 6 decimals
  console.log(removed ? 'REVERTED' : 'received', Number(BigInt(data.value)) / 1e6, 'USDT from', data.from);
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
                "topic": "erc20Transfers",
                "filters": {
                    "tokenContract": "0xdAC17F958D2ee523a2206206994597C13D831ec7",
                    "to": "0xa2050435e59f41300b7787808e976255faa198e5"
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
            # value is in the token's smallest unit; USDT has 6 decimals
            print('REVERTED' if removed else 'received', int(data['value'], 16) / 1e6, 'USDT from', data['from'])

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
  "result": "0x573e3382e8ea7d534f267b232aae22dd"
}
```

Every later message is an event notification:

```json
{
  "jsonrpc": "2.0",
  "method": "getblock_subscribe",
  "params": {
    "subscription": "0x573e3382e8ea7d534f267b232aae22dd",
    "result": {
      "eventId": "eth:mainnet:receipts:0x19ddc6864bd5ec1cdbd493e3d3653b529d79293a677…",
      "removed": false,
      "revision": 116876401,
      "data": {
        "tokenContract": "0xdac17f958d2ee523a2206206994597c13d831ec7",
        "from": "0x4b3dd3690ba06b2a73089e02381d0eba9a7dbca5",
        "to": "0xa2050435e59f41300b7787808e976255faa198e5",
        "value": "0x6f4b360",
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
<tr><td><code>data.tokenContract</code></td><td>string</td><td>ERC-20 token contract address.</td></tr>
<tr><td><code>data.from</code></td><td>string</td><td>Sender. <code>0x000…000</code> for a mint.</td></tr>
<tr><td><code>data.to</code></td><td>string</td><td>Recipient. <code>0x000…000</code> for a burn.</td></tr>
<tr><td><code>data.value</code></td><td>string</td><td>Amount in the token's smallest unit, hex-encoded. Divide by <code>10^decimals</code>; the token's <code>decimals()</code> is not included.</td></tr>
<tr><td><code>data.address</code></td><td>string</td><td>Address of the contract that emitted the log.</td></tr>
<tr><td><code>data.blockNumber</code></td><td>string</td><td>Block number, hex-encoded.</td></tr>
<tr><td><code>data.blockHash</code></td><td>string</td><td>Hash of the block that contains the log.</td></tr>
<tr><td><code>data.blockTimestamp</code></td><td>string</td><td>Block timestamp in Unix seconds, hex-encoded.</td></tr>
<tr><td><code>data.transactionHash</code></td><td>string</td><td>Hash of the transaction that emitted the log.</td></tr>
<tr><td><code>data.transactionIndex</code></td><td>string</td><td>Transaction's position in the block, hex-encoded.</td></tr>
<tr><td><code>data.logIndex</code></td><td>string</td><td>Log's position in the block, hex-encoded.</td></tr>
<tr><td><code>data.removed</code></td><td>boolean</td><td>The underlying log's <code>removed</code> flag. Use the envelope's <code>removed</code> for reorg handling.</td></tr>
</tbody></table>

{% hint style="info" %}
A transaction that moves tokens several times (a swap route, for example) produces one notification per `Transfer` log. Use `transactionHash` + `logIndex`, or the `eventId`, to tell them apart.
{% endhint %}

## Use Cases

* Detecting incoming payments to a deposit address
* Wallet activity feeds and push notifications
* Treasury, exchange, and whale-movement monitoring
* Tracking mints and burns of a stablecoin (filter `from` or `to` on the zero address)

## Error Handling

<table data-search="false"><thead><tr><th>Code</th><th>Message</th><th>Cause</th></tr></thead><tbody>
<tr><td><code>-32602</code></td><td><code>invalid tokenContract: invalid EVM address "…"</code></td><td><code>tokenContract</code> is not a 20-byte hex address.</td></tr>
<tr><td><code>-32602</code></td><td><code>invalid from: invalid EVM address "…"</code></td><td><code>from</code> is not a 20-byte hex address.</td></tr>
<tr><td><code>-32602</code></td><td><code>invalid to: invalid EVM address "…"</code></td><td><code>to</code> is not a 20-byte hex address.</td></tr>
<tr><td><code>-32602</code></td><td><code>invalid erc20Transfers request: json: unknown field "…"</code></td><td>The request has a field this topic does not accept.</td></tr>
<tr><td><code>-32001</code></td><td><code>subscription limit exceeded</code></td><td>You already have 15 active or pending subscriptions.</td></tr>
</tbody></table>
