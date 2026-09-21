---
description: >-
  Stream every new full block, with transaction objects, over WebSocket. Complete guide on how to use the newBlocks topic in GetBlock EVM Data Stream documentation.
---

# newBlocks - EVM Data Stream

The `newBlocks` topic sends one notification per new block, carrying the **full block** with every transaction as a complete object. Use it when you index everything in a block and would otherwise call `eth_getBlockByNumber(…, true)` after each header.

{% hint style="warning" %}
**WebSocket-only topic.** Subscribe with [`getblock_subscribe`](getblock_subscribe-evm-data-stream.md) on any [network endpoint](./#endpoints).
{% endhint %}

{% hint style="info" %}
Full blocks are large: an Ethereum block is often several hundred kilobytes of JSON. If you only need a block signal, use [`newHeads`](newheads-evm-data-stream.md). If you only need some transactions, use [`newMinedTransactions`](newminedtransactions-evm-data-stream.md) with address rules.
{% endhint %}

## Parameters

`newBlocks` accepts no parameters. Send only the topic name; any other field is rejected.

## Request Example

{% tabs %}
{% tab title="wscat" %}
{% code overflow="wrap" %}
```bash
# WebSocket-only. Connect first:
wscat -c 'wss://stream.eu-central-1.getblock.io/v1/eth-mainnet/stream' -H 'Authorization: Bearer <API-KEY>'

# Then send:
{"jsonrpc":"2.0","id":1,"method":"getblock_subscribe","params":[{"topic":"newBlocks"}]}
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
      topic: 'newBlocks'
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
  console.log(removed ? 'REMOVED' : 'block', parseInt(data.number, 16), `${data.transactions.length} txs`);
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
                "topic": "newBlocks"
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
            print('REMOVED' if removed else 'block', int(data['number'], 16), len(data['transactions']), 'txs')

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
  "result": "0x5f020d44c27b974318b8da1cc04c3e75"
}
```

Every later message is an event notification (transactions and withdrawals shortened to one entry each):

```json
{
  "jsonrpc": "2.0",
  "method": "getblock_subscribe",
  "params": {
    "subscription": "0x5f020d44c27b974318b8da1cc04c3e75",
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
        "size": "0xb299",
        "stateRoot": "0xf2ffbd7a38983140a6719376f64b4d258a35c9dfd176c613c4d9f57a50b27de6",
        "timestamp": "0x6ab110c3",
        "transactions": [
          {
            "blockHash": "0x19ddc6864bd5ec1cdbd493e3d3653b529d79293a6773517018153cdff6ac0be2",
            "blockNumber": "0x18d1e1e",
            "blockTimestamp": "0x6ab110c3",
            "from": "0x341beb8ac39ec376182d56b358bcaa45e65d31b9",
            "gas": "0xb1f0",
            "gasPrice": "0xcbdbda20",
            "maxFeePerGas": "0xe6e2184e",
            "maxPriorityFeePerGas": "0xb2d05e00",
            "hash": "0xba13a33781a3c6b8c344eb183db35a3dab9886c651683537013558676e121dc0",
            "input": "0xa9059cbb000000000000000000000000b216a6a4fae94ac549916652bcb3bced4e6b15be00000000000000000000000000000000000000000000000000000004b693aac0",
            "nonce": "0x30ac",
            "to": "0xa0b86991c6218b36c1d19d4a2e9eb0ce3606eb48",
            "transactionIndex": "0x0",
            "value": "0x0",
            "type": "0x2",
            "accessList": [],
            "chainId": "0x1",
            "v": "0x0",
            "r": "0x5c649f399f6fec0cc3455bc0ba4043c1829267d5bc332385c54f7187f66d3214",
            "s": "0x5a59d03e85dd7818ac906575597ab612efd239b815dd16eb8d5e272aebe108b7",
            "yParity": "0x0"
          },
          "…"
        ],
        "transactionsRoot": "0xffc6c73100603b7f9406aca781edc8034d25e6f0d288113f7b32a3312ebd75a2",
        "uncles": [],
        "withdrawals": [
          {
            "index": "0x892f4cb",
            "validatorIndex": "0x7a0c3",
            "address": "0xa51e042ca04b7d7555eb94c51692e3a98c09c322",
            "amount": "0xda1008"
          },
          "…"
        ],
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
<tr><td><code>data</code></td><td>object</td><td>Full block, in the same shape as <code>eth_getBlockByNumber(…, true)</code>. It has every [<code>newHeads</code>](newheads-evm-data-stream.md) header field plus the fields below.</td></tr>
<tr><td><code>data.transactions</code></td><td>array</td><td>Every transaction in the block as a full object, in block order. Same shape as the <code>transaction</code> object in [<code>newMinedTransactions</code>](newminedtransactions-evm-data-stream.md#response-parameters).</td></tr>
<tr><td><code>data.withdrawals</code></td><td>array</td><td>Validator withdrawals processed in the block, each with <code>index</code>, <code>validatorIndex</code>, <code>address</code>, and <code>amount</code> (in gwei, hex-encoded). Present where the network has withdrawals.</td></tr>
<tr><td><code>data.uncles</code></td><td>array</td><td>Uncle block hashes. Always empty on proof-of-stake Ethereum.</td></tr>
<tr><td><code>data.size</code></td><td>string</td><td>Block size in bytes, hex-encoded.</td></tr>
</tbody></table>

## Use Cases

* Indexing every transaction on a network without follow-up RPC calls
* Block explorers and analytics pipelines
* Computing per-block statistics such as transaction count, fees, and top senders
* Keeping a local database in sync with the chain tip

## Error Handling

<table data-search="false"><thead><tr><th>Code</th><th>Message</th><th>Cause</th></tr></thead><tbody>
<tr><td><code>-32602</code></td><td><code>invalid newBlocks request: json: unknown field "…"</code></td><td><code>newBlocks</code> takes no parameters.</td></tr>
<tr><td><code>-32602</code></td><td><code>invalid newBlocks request: json: unknown field "…"</code></td><td>The request has a field this topic does not accept.</td></tr>
<tr><td><code>-32001</code></td><td><code>subscription limit exceeded</code></td><td>You already have 15 active or pending subscriptions.</td></tr>
</tbody></table>
