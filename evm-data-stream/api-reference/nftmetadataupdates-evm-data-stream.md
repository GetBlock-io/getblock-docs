---
description: >-
  Stream decoded ERC-4906 MetadataUpdate and BatchMetadataUpdate events. Complete guide on how to use the nftMetadataUpdates topic in GetBlock EVM Data Stream documentation.
---

# nftMetadataUpdates - EVM Data Stream

The `nftMetadataUpdates` topic sends decoded [ERC-4906](https://eips.ethereum.org/EIPS/eip-4906) events. Collections emit these to announce that a token's metadata changed, so indexers and marketplaces know to refresh `tokenURI` instead of polling it.

{% hint style="warning" %}
**WebSocket-only topic.** Subscribe with [`getblock_subscribe`](getblock_subscribe-evm-data-stream.md) on any [network endpoint](./#endpoints).
{% endhint %}

## Parameters

<table data-search="false"><thead><tr><th>Parameter</th><th>Type</th><th>Required</th><th>Description</th></tr></thead><tbody>
<tr><td><code>filters.tokenContract</code></td><td>string</td><td>No</td><td>NFT contract address.</td></tr>
<tr><td><code>filters.tokenId</code></td><td>string</td><td>No</td><td>One token ID as a hex quantity.</td></tr>
</tbody></table>

## Request Example

{% tabs %}
{% tab title="wscat" %}
{% code overflow="wrap" %}
```bash
# WebSocket-only. Connect first:
wscat -c 'wss://stream.eu-central-1.getblock.io/v1/eth-mainnet/stream' -H 'Authorization: Bearer <API-KEY>'

# Then send:
{"jsonrpc":"2.0","id":1,"method":"getblock_subscribe","params":[{"topic":"nftMetadataUpdates","filters":{"tokenContract":"0x8004a169fb4a3325136eb29fa0ceb6d2e539a432"}}]}
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
      topic: 'nftMetadataUpdates',
      filters: {
        tokenContract: '0x8004a169fb4a3325136eb29fa0ceb6d2e539a432'
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
  if (!removed) console.log('refresh metadata', data.tokenContract, data.updateType, data.tokenId);
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
                "topic": "nftMetadataUpdates",
                "filters": {
                    "tokenContract": "0x8004a169fb4a3325136eb29fa0ceb6d2e539a432"
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
            if not removed:
                print('refresh metadata', data['tokenContract'], data['updateType'], data.get('tokenId'))

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
  "result": "0xc37755570546333df386be6355ee736e"
}
```

Every later message is an event notification:

```json
{
  "jsonrpc": "2.0",
  "method": "getblock_subscribe",
  "params": {
    "subscription": "0xc37755570546333df386be6355ee736e",
    "result": {
      "eventId": "eth:mainnet:receipts:0x5cb2b901e6fd1e2ca3a7cef95af7a22946a4d973840…",
      "removed": false,
      "revision": 117013229,
      "data": {
        "updateType": "single",
        "tokenContract": "0x8004a169fb4a3325136eb29fa0ceb6d2e539a432",
        "tokenId": "0xc748",
        "address": "0x8004a169fb4a3325136eb29fa0ceb6d2e539a432",
        "blockNumber": "0x18d1e8e",
        "transactionHash": "0x05b874945b5f64705afea62f0063c423705c56ea35f03e565dffef95e43ddacb",
        "transactionIndex": "0x2b",
        "blockHash": "0x5cb2b901e6fd1e2ca3a7cef95af7a22946a4d973840eecbc047574245161d112",
        "blockTimestamp": "0x6ab11603",
        "logIndex": "0xe0",
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
<tr><td><code>data.updateType</code></td><td>string</td><td><code>single</code> for <code>MetadataUpdate(tokenId)</code>. A <code>BatchMetadataUpdate(fromTokenId, toTokenId)</code> covers a range of tokens instead of one.</td></tr>
<tr><td><code>data.tokenContract</code></td><td>string</td><td>NFT contract address.</td></tr>
<tr><td><code>data.tokenId</code></td><td>string</td><td>For <code>single</code> updates: the token whose metadata changed, hex-encoded.</td></tr>
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
ERC-4906 events are relatively rare: only collections that implement the standard emit them. Expect long gaps between notifications on an unfiltered subscription.
{% endhint %}

## Use Cases

* Invalidating cached NFT metadata and images
* Tracking reveals, upgrades, and evolving or dynamic NFTs
* Keeping marketplace listings in sync with on-chain metadata changes

## Error Handling

<table data-search="false"><thead><tr><th>Code</th><th>Message</th><th>Cause</th></tr></thead><tbody>
<tr><td><code>-32602</code></td><td><code>… cannot unmarshal hex string without 0x prefix …</code></td><td>A hex-quantity filter was sent as a decimal string. Encode it as <code>0x…</code>.</td></tr>
<tr><td><code>-32602</code></td><td><code>invalid tokenContract: invalid EVM address "…"</code></td><td><code>tokenContract</code> is not a 20-byte hex address.</td></tr>
<tr><td><code>-32602</code></td><td><code>invalid nftMetadataUpdates request: json: unknown field "…"</code></td><td>The request has a field this topic does not accept.</td></tr>
<tr><td><code>-32001</code></td><td><code>subscription limit exceeded</code></td><td>You already have 15 active or pending subscriptions.</td></tr>
</tbody></table>
