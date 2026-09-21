---
description: >-
  Stream every new block header over WebSocket. Complete guide on how to use the newHeads topic in GetBlock EVM Data Stream documentation.
---

# newHeads - EVM Data Stream

The `newHeads` topic sends one notification per new block, carrying the block **header**: number, hash, parent hash, gas, base fee, and state roots, but no transactions. It is the lightest way to follow the chain tip.

{% hint style="warning" %}
**WebSocket-only topic.** Subscribe with [`getblock_subscribe`](getblock_subscribe-evm-data-stream.md) on any [network endpoint](./#endpoints).
{% endhint %}

## Parameters

`newHeads` accepts no parameters. Send only the topic name; any other field, including an empty `filters` object, is rejected.

## Request Example

{% tabs %}
{% tab title="wscat" %}
{% code overflow="wrap" %}
```bash
# WebSocket-only. Connect first:
wscat -c 'wss://stream.eu-central-1.getblock.io/v1/eth-mainnet/stream' -H 'Authorization: Bearer <API-KEY>'

# Then send:
{"jsonrpc":"2.0","id":1,"method":"getblock_subscribe","params":[{"topic":"newHeads"}]}
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
      topic: 'newHeads'
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
  console.log(removed ? 'REMOVED' : 'block', parseInt(data.number, 16), data.hash);
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
                "topic": "newHeads"
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
            print('REMOVED' if removed else 'block', int(data['number'], 16), data['hash'])

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
  "result": "0xfd5126364b7ac0fbe8ad9099ce29571c"
}
```

Every later message is an event notification:

```json
{
  "jsonrpc": "2.0",
  "method": "getblock_subscribe",
  "params": {
    "subscription": "0xfd5126364b7ac0fbe8ad9099ce29571c",
    "result": {
      "eventId": "eth:mainnet:block:0x19ddc6864bd5ec1cdbd493e3d3653b529d79293a677351…",
      "removed": false,
      "revision": 116876399,
      "data": {
        "baseFeePerGas": "0x190b7c20",
        "blobGasUsed": "0x40000",
        "difficulty": "0x0",
        "excessBlobGas": "0xbdce5c2",
        "extraData": "0xd883011007846765746888676f312e32352e31856c696e7578",
        "gasLimit": "0x3938700",
        "gasUsed": "0x693610",
        "hash": "0x19ddc6864bd5ec1cdbd493e3d3653b529d79293a6773517018153cdff6ac0be2",
        "logsBloom": "0x02c800420c4a08832003402000001e00711800280420105000000472c0820204…",
        "miner": "0x24d6c74d811cfde65995ed26fd08af445f8aab06",
        "mixHash": "0xeefe827e31ba13e7f34e498f2ae3d25cc4313b9179f160e88217d3bae5de056f",
        "nonce": "0x0000000000000000",
        "number": "0x18d1e1e",
        "parentBeaconBlockRoot": "0x5c220c17a3e9f080b1cf47a399f0d2223c128d6faf7c7c3a4ea6e4cfcd0ef407",
        "parentHash": "0x31e49d2ea3bce6863b0c09b3aa0b9b5c274349b54387a2ef4e61f2bc01e72f93",
        "receiptsRoot": "0x9ec6961f68faefa809ef5f260dd37c8c42e553cfe17da4858b4a61435c3a1be2",
        "requestsHash": "0xe3b0c44298fc1c149afbf4c8996fb92427ae41e4649b934ca495991b7852b855",
        "sha3Uncles": "0x1dcc4de8dec75d7aab85b567b6ccd41ad312451b948a7413f0a142fd40d49347",
        "stateRoot": "0xf2ffbd7a38983140a6719376f64b4d258a35c9dfd176c613c4d9f57a50b27de6",
        "timestamp": "0x6ab110c3",
        "transactionsRoot": "0xffc6c73100603b7f9406aca781edc8034d25e6f0d288113f7b32a3312ebd75a2",
        "withdrawalsRoot": "0x0746d437d65f781208c50939138cf81cecc6bd2922263e892f1e6dd9d865b0f5"
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
<tr><td><code>data</code></td><td>object</td><td>Block header, in the same shape as <code>eth_getBlockByNumber</code> without <code>transactions</code>, <code>uncles</code>, <code>withdrawals</code>, or <code>size</code>.</td></tr>
<tr><td><code>data.number</code></td><td>string</td><td>Block number, hex-encoded.</td></tr>
<tr><td><code>data.hash</code></td><td>string</td><td>Block hash.</td></tr>
<tr><td><code>data.parentHash</code></td><td>string</td><td>Hash of the parent block.</td></tr>
<tr><td><code>data.timestamp</code></td><td>string</td><td>Block timestamp in Unix seconds, hex-encoded.</td></tr>
<tr><td><code>data.miner</code></td><td>string</td><td>Fee recipient (block producer) address.</td></tr>
<tr><td><code>data.gasLimit</code></td><td>string</td><td>Block gas limit, hex-encoded.</td></tr>
<tr><td><code>data.gasUsed</code></td><td>string</td><td>Gas used by all transactions in the block, hex-encoded.</td></tr>
<tr><td><code>data.baseFeePerGas</code></td><td>string</td><td>EIP-1559 base fee in wei, hex-encoded.</td></tr>
<tr><td><code>data.stateRoot</code></td><td>string</td><td>Root of the state trie after this block.</td></tr>
<tr><td><code>data.transactionsRoot</code></td><td>string</td><td>Root of the transactions trie.</td></tr>
<tr><td><code>data.receiptsRoot</code></td><td>string</td><td>Root of the receipts trie.</td></tr>
<tr><td><code>data.logsBloom</code></td><td>string</td><td>Bloom filter for the logs in the block.</td></tr>
<tr><td><code>data.extraData</code></td><td>string</td><td>Arbitrary data set by the block producer.</td></tr>
<tr><td><code>data.difficulty</code></td><td>string</td><td>Block difficulty. <code>0x0</code> on proof-of-stake Ethereum.</td></tr>
<tr><td><code>data.nonce</code></td><td>string</td><td>Proof-of-work nonce. Zero on proof-of-stake Ethereum.</td></tr>
<tr><td><code>data.mixHash</code></td><td>string</td><td>Beacon-chain randomness (<code>prevRandao</code>) on proof-of-stake Ethereum.</td></tr>
<tr><td><code>data.sha3Uncles</code></td><td>string</td><td>Hash of the uncles list.</td></tr>
<tr><td><code>data.withdrawalsRoot</code></td><td>string</td><td>Root of the validator withdrawals list, where the network has withdrawals.</td></tr>
<tr><td><code>data.blobGasUsed</code></td><td>string</td><td>EIP-4844 blob gas used, where supported.</td></tr>
<tr><td><code>data.excessBlobGas</code></td><td>string</td><td>EIP-4844 excess blob gas, where supported.</td></tr>
<tr><td><code>data.parentBeaconBlockRoot</code></td><td>string</td><td>EIP-4788 parent beacon block root, where supported.</td></tr>
<tr><td><code>data.requestsHash</code></td><td>string</td><td>EIP-7685 execution-layer requests hash, where supported.</td></tr>
</tbody></table>

{% hint style="info" %}
The header fields follow each network's client. Fork-specific fields such as `blobGasUsed` or `requestsHash` appear only where that network has activated the fork, and some networks add fields of their own. Ignore fields you don't use.
{% endhint %}

## Use Cases

* Triggering work once per block, such as refreshing balances or re-quoting prices
* Monitoring chain liveness and block times
* Tracking gas usage and base fee for fee estimation
* Detecting reorgs at the header level (Ethereum and Polygon)

## Error Handling

<table data-search="false"><thead><tr><th>Code</th><th>Message</th><th>Cause</th></tr></thead><tbody>
<tr><td><code>-32602</code></td><td><code>invalid newHeads request: json: unknown field "filters"</code></td><td><code>newHeads</code> takes no parameters, not even an empty <code>filters</code> object.</td></tr>
<tr><td><code>-32602</code></td><td><code>invalid newHeads request: json: unknown field "…"</code></td><td>The request has a field this topic does not accept.</td></tr>
<tr><td><code>-32001</code></td><td><code>subscription limit exceeded</code></td><td>You already have 15 active or pending subscriptions.</td></tr>
</tbody></table>
