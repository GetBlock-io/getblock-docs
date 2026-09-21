---
description: >-
  Stream pending mempool transactions, as full objects or hashes, filtered by sender and recipient. Complete guide on how to use the newPendingTransactions topic in GetBlock EVM Data Stream documentation.
---

# newPendingTransactions - EVM Data Stream

The `newPendingTransactions` topic sends transactions **observed in the mempool before they are mined**, as full objects or hashes only. Use it to react to transactions before inclusion, such as showing an incoming payment as pending.

{% hint style="warning" %}
**WebSocket-only topic.** Subscribe with [`getblock_subscribe`](getblock_subscribe-evm-data-stream.md) on any [network endpoint](./#endpoints).
{% endhint %}

{% hint style="info" %}
Pending transactions are what GetBlock's nodes observe in the public mempool. Transactions sent through private relays never appear here, and a pending transaction may be replaced, dropped, or never mined. Mempool availability depends on the network.
{% endhint %}

## Parameters

<table data-search="false"><thead><tr><th>Parameter</th><th>Type</th><th>Required</th><th>Description</th></tr></thead><tbody>
<tr><td><code>hashesOnly</code></td><td>boolean</td><td>No</td><td><code>true</code> to receive only the transaction hash. Default <code>false</code>: the full transaction object.</td></tr>
<tr><td><code>fromAddress</code></td><td>string or array</td><td>No</td><td>Sender address, or an array of senders.</td></tr>
<tr><td><code>toAddress</code></td><td>string or array</td><td>No</td><td>Recipient address, or an array of recipients.</td></tr>
</tbody></table>

These fields sit at the **top level** of the request object, not inside `filters`. Address matching is **OR**: a transaction matches if its sender is in `fromAddress` **or** its recipient is in `toAddress`. `fromAddress` and `toAddress` together accept at most 50 addresses.

## Request Example

{% tabs %}
{% tab title="wscat" %}
{% code overflow="wrap" %}
```bash
# WebSocket-only. Connect first:
wscat -c 'wss://stream.eu-central-1.getblock.io/v1/eth-mainnet/stream' -H 'Authorization: Bearer <API-KEY>'

# Then send:
{"jsonrpc":"2.0","id":1,"method":"getblock_subscribe","params":[{"topic":"newPendingTransactions","hashesOnly":false,"toAddress":["0xdAC17F958D2ee523a2206206994597C13D831ec7"]}]}
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
      topic: 'newPendingTransactions',
      hashesOnly: false,
      toAddress: [
        '0xdAC17F958D2ee523a2206206994597C13D831ec7'
      ]
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
  // data is the transaction object, or a hash string with hashesOnly
  const hash = typeof data === 'string' ? data : data.hash;
  console.log('pending', hash);
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
                "topic": "newPendingTransactions",
                "hashesOnly": False,
                "toAddress": [
                    "0xdAC17F958D2ee523a2206206994597C13D831ec7"
                ]
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
            # data is the transaction object, or a hash string with hashesOnly
            tx_hash = data if isinstance(data, str) else data['hash']
            print('pending', tx_hash)

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
  "result": "0xb804f790318735a54af48a0bd5e2a722"
}
```

Every later message is an event notification:

```json
{
  "jsonrpc": "2.0",
  "method": "getblock_subscribe",
  "params": {
    "subscription": "0xb804f790318735a54af48a0bd5e2a722",
    "result": {
      "eventId": "eth:mainnet:pending_tx:0xe0edbc80b18c6ca13ad9fda66cc84d439df2f07c5…",
      "removed": false,
      "revision": 117006720,
      "data": {
        "blockHash": null,
        "blockNumber": null,
        "blockTimestamp": null,
        "from": "0x559432e18b281731c054cd703d4b49872be4ed53",
        "gas": "0x33450",
        "gasPrice": "0x74ce93d080",
        "maxFeePerGas": "0x74ce93d080",
        "maxPriorityFeePerGas": "0x64414880",
        "hash": "0xe0edbc80b18c6ca13ad9fda66cc84d439df2f07c5fdffed9887d242eb45c0dea",
        "input": "0xa9059cbb000000000000000000000000a7e42c09b68b35f9bf94a7b78384515239d3f3ab0000000000000000000000000000000000000000000000000000000002b5c960",
        "nonce": "0x72da69",
        "to": "0xdac17f958d2ee523a2206206994597c13d831ec7",
        "transactionIndex": null,
        "value": "0x0",
        "type": "0x2",
        "accessList": [],
        "chainId": "0x1",
        "v": "0x0",
        "r": "0x1d5d4283207c53538d4cf8a9b7d7cc5ea879068f461a25bc0865fa463a5806a4",
        "s": "0x1ddaaa9101d2ff821dce7e4b3a670090f0ef42b06c7de6aa35ee33ab9a13174f",
        "yParity": "0x0"
      }
    }
  }
}
```

With `"hashesOnly": true`, `data` is the hash string itself:

```json
{
  "eventId": "eth:mainnet:pending_tx:0x89cdc9983780a9e1c207351f10e35f697a30b0dd54855605e1f7f0e92fd25e15:newPendingTransactions:0x89cdc9983780a9e1c207351f10e35f697a30b0dd54855605e1f7f0e92fd25e15",
  "removed": false,
  "revision": 117081345,
  "data": "0x89cdc9983780a9e1c207351f10e35f697a30b0dd54855605e1f7f0e92fd25e15"
}
```

{% hint style="warning" %}
The payload shape differs from `newMinedTransactions`. Here `data` **is** the transaction object (or hash string); it is not wrapped in `data.transaction`.
{% endhint %}

## Response Parameters

The `result` object contains the event envelope and the payload in `data`.

<table data-search="false"><thead><tr><th>Field</th><th>Type</th><th>Description</th></tr></thead><tbody>
<tr><td><code>eventId</code></td><td>string</td><td>Stable event identifier. A reorg correction reuses it.</td></tr>
<tr><td><code>removed</code></td><td>boolean</td><td><code>true</code> when a reorg removed this previously delivered event.</td></tr>
<tr><td><code>revision</code></td><td>number</td><td>Event version. For one <code>eventId</code>, a greater revision is newer.</td></tr>
<tr><td><code>data</code></td><td>object or string</td><td>The pending transaction, in the same shape as <code>eth_getTransactionByHash</code>. With <code>hashesOnly</code>, the transaction hash as a string.</td></tr>
<tr><td><code>data.hash</code></td><td>string</td><td>Transaction hash.</td></tr>
<tr><td><code>data.from</code></td><td>string</td><td>Sender.</td></tr>
<tr><td><code>data.to</code></td><td>string</td><td>Recipient. <code>null</code> for contract creation.</td></tr>
<tr><td><code>data.value</code></td><td>string</td><td>Native value in wei, hex-encoded.</td></tr>
<tr><td><code>data.input</code></td><td>string</td><td>Call data.</td></tr>
<tr><td><code>data.nonce</code></td><td>string</td><td>Sender nonce, hex-encoded.</td></tr>
<tr><td><code>data.gas</code></td><td>string</td><td>Gas limit, hex-encoded.</td></tr>
<tr><td><code>data.gasPrice</code></td><td>string</td><td>Gas price in wei, hex-encoded. Equals <code>maxFeePerGas</code> for EIP-1559 transactions.</td></tr>
<tr><td><code>data.maxFeePerGas</code></td><td>string</td><td>EIP-1559 max fee per gas.</td></tr>
<tr><td><code>data.maxPriorityFeePerGas</code></td><td>string</td><td>EIP-1559 priority fee per gas.</td></tr>
<tr><td><code>data.type</code></td><td>string</td><td>EIP-2718 transaction type, hex-encoded.</td></tr>
<tr><td><code>data.chainId</code></td><td>string</td><td>Chain ID, hex-encoded.</td></tr>
<tr><td><code>data.accessList</code></td><td>array</td><td>EIP-2930 access list.</td></tr>
<tr><td><code>data.blockHash, blockNumber, blockTimestamp, transactionIndex</code></td><td>null</td><td>Always <code>null</code>: the transaction is not in a block yet.</td></tr>
<tr><td><code>data.v, r, s, yParity</code></td><td>string</td><td>Signature values.</td></tr>
</tbody></table>

## Use Cases

* Showing incoming payments as "pending" before confirmation
* Monitoring your own broadcast transactions for replacement or stalling
* Mempool analytics and gas-price research
* Detecting pending interactions with a contract you operate

## Error Handling

<table data-search="false"><thead><tr><th>Code</th><th>Message</th><th>Cause</th></tr></thead><tbody>
<tr><td><code>-32602</code></td><td><code>fromAddress and toAddress exceed the 50-address limit</code></td><td>More than 50 addresses in total.</td></tr>
<tr><td><code>-32602</code></td><td><code>invalid toAddress address "…"</code></td><td>An address is not a 20-byte hex address. The same applies to <code>fromAddress</code>.</td></tr>
<tr><td><code>-32602</code></td><td><code>invalid newPendingTransactions request: json: unknown field "filters"</code></td><td>The fields were nested in <code>filters</code>. Put them at the top level.</td></tr>
<tr><td><code>-32602</code></td><td><code>invalid newPendingTransactions request: json: unknown field "…"</code></td><td>The request has a field this topic does not accept.</td></tr>
<tr><td><code>-32001</code></td><td><code>subscription limit exceeded</code></td><td>You already have 15 active or pending subscriptions.</td></tr>
</tbody></table>
