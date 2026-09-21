---
description: >-
  Stream transactions as they are included in blocks, as full objects or hashes, filtered by from/to address rules. Complete guide on how to use the newMinedTransactions topic in GetBlock EVM Data Stream documentation.
---

# newMinedTransactions - EVM Data Stream

The `newMinedTransactions` topic sends one notification per transaction **included in a new block**, either as the full transaction object or as its hash only. Address rules let you receive just the transactions of the wallets or contracts you care about.

{% hint style="warning" %}
**WebSocket-only topic.** Subscribe with [`getblock_subscribe`](getblock_subscribe-evm-data-stream.md) on any [network endpoint](./#endpoints).
{% endhint %}

## Parameters

<table data-search="false"><thead><tr><th>Parameter</th><th>Type</th><th>Required</th><th>Description</th></tr></thead><tbody>
<tr><td><code>hashesOnly</code></td><td>boolean</td><td>No</td><td><code>true</code> to receive only the transaction hash. Default <code>false</code>: the full transaction object.</td></tr>
<tr><td><code>addresses</code></td><td>array</td><td>No</td><td>Up to 50 match rules, each <code>{ "from": "0x…", "to": "0x…" }</code> with at least one of the two set.</td></tr>
</tbody></table>

These fields sit at the **top level** of the request object, next to `topic`, not inside `filters`.

How `addresses` rules combine: fields inside one rule are **AND**, and separate rules are **OR**.

```json
"addresses": [
  { "from": "0xA…", "to": "0xB…" },
  { "to": "0xC…" }
]
```

This matches transactions sent from `0xA…` to `0xB…`, plus every transaction sent to `0xC…`. Omit `addresses` to receive every transaction in every block.

## Request Example

{% tabs %}
{% tab title="wscat" %}
{% code overflow="wrap" %}
```bash
# WebSocket-only. Connect first:
wscat -c 'wss://stream.eu-central-1.getblock.io/v1/eth-mainnet/stream' -H 'Authorization: Bearer <API-KEY>'

# Then send:
{"jsonrpc":"2.0","id":1,"method":"getblock_subscribe","params":[{"topic":"newMinedTransactions","hashesOnly":false,"addresses":[{"to":"0xdAC17F958D2ee523a2206206994597C13D831ec7"}]}]}
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
      topic: 'newMinedTransactions',
      hashesOnly: false,
      addresses: [
        {
          to: '0xdAC17F958D2ee523a2206206994597C13D831ec7'
        }
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
  const tx = data.transaction;
  console.log(removed ? 'REVERTED' : 'mined', tx.hash, 'block', parseInt(tx.blockNumber, 16));
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
                "topic": "newMinedTransactions",
                "hashesOnly": False,
                "addresses": [
                    {
                        "to": "0xdAC17F958D2ee523a2206206994597C13D831ec7"
                    }
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
            tx = data['transaction']
            print('REVERTED' if removed else 'mined', tx['hash'], 'block', int(tx['blockNumber'], 16))

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
  "result": "0x09e2b35d6f3a73734f8e162b2aca6991"
}
```

Every later message is an event notification:

```json
{
  "jsonrpc": "2.0",
  "method": "getblock_subscribe",
  "params": {
    "subscription": "0x09e2b35d6f3a73734f8e162b2aca6991",
    "result": {
      "eventId": "eth:mainnet:block:0x19ddc6864bd5ec1cdbd493e3d3653b529d79293a677351…",
      "removed": false,
      "revision": 116876399,
      "data": {
        "removed": false,
        "transaction": {
          "blockHash": "0x19ddc6864bd5ec1cdbd493e3d3653b529d79293a6773517018153cdff6ac0be2",
          "blockNumber": "0x18d1e1e",
          "blockTimestamp": "0x6ab110c3",
          "from": "0x4b3dd3690ba06b2a73089e02381d0eba9a7dbca5",
          "gas": "0xb435",
          "gasPrice": "0x90411020",
          "maxFeePerGas": "0x919e1a15",
          "maxPriorityFeePerGas": "0x77359400",
          "hash": "0x6b362543c83c1d18b27701235338c05a7e04c2e5098bed46d60afe9a0f17d2a1",
          "input": "0xa9059cbb000000000000000000000000a2050435e59f41300b7787808e976255faa198e50000000000000000000000000000000000000000000000000000000006f4b360",
          "nonce": "0x0",
          "to": "0xdac17f958d2ee523a2206206994597c13d831ec7",
          "transactionIndex": "0x3",
          "value": "0x0",
          "type": "0x2",
          "accessList": [],
          "chainId": "0x1",
          "v": "0x1",
          "r": "0x5c77a499118778830cb1b24a41b6c9a83b191f310f96759cf91f220557baa58f",
          "s": "0x34bc833d1d7e796b12a577978f589ec06e22781cc6e71d1245c566c5506e8276",
          "yParity": "0x1"
        }
      }
    }
  }
}
```

With `"hashesOnly": true`, `transaction` holds only the hash:

```json
{
  "data": {
    "removed": false,
    "transaction": {
      "hash": "0x9b4585ad4e6af637077628e2c43196fb91b8318758ae3a4409b61b6c7c325833"
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
<tr><td><code>data.removed</code></td><td>boolean</td><td>Mirrors the envelope's <code>removed</code>. Use the envelope field for reorg handling.</td></tr>
<tr><td><code>data.transaction</code></td><td>object</td><td>The transaction, in the same shape as <code>eth_getTransactionByHash</code>. With <code>hashesOnly</code>, only <code>hash</code>.</td></tr>
<tr><td><code>data.transaction.hash</code></td><td>string</td><td>Transaction hash.</td></tr>
<tr><td><code>data.transaction.blockHash</code></td><td>string</td><td>Hash of the block that included the transaction.</td></tr>
<tr><td><code>data.transaction.blockNumber</code></td><td>string</td><td>Block number, hex-encoded.</td></tr>
<tr><td><code>data.transaction.blockTimestamp</code></td><td>string</td><td>Block timestamp in Unix seconds, hex-encoded.</td></tr>
<tr><td><code>data.transaction.transactionIndex</code></td><td>string</td><td>Transaction's position in the block, hex-encoded.</td></tr>
<tr><td><code>data.transaction.from</code></td><td>string</td><td>Sender.</td></tr>
<tr><td><code>data.transaction.to</code></td><td>string</td><td>Recipient. <code>null</code> for contract creation.</td></tr>
<tr><td><code>data.transaction.value</code></td><td>string</td><td>Native value transferred in wei, hex-encoded.</td></tr>
<tr><td><code>data.transaction.input</code></td><td>string</td><td>Call data.</td></tr>
<tr><td><code>data.transaction.nonce</code></td><td>string</td><td>Sender nonce, hex-encoded.</td></tr>
<tr><td><code>data.transaction.gas</code></td><td>string</td><td>Gas limit, hex-encoded.</td></tr>
<tr><td><code>data.transaction.gasPrice</code></td><td>string</td><td>Effective gas price in wei, hex-encoded.</td></tr>
<tr><td><code>data.transaction.maxFeePerGas</code></td><td>string</td><td>EIP-1559 max fee per gas, on type <code>0x2</code> and later.</td></tr>
<tr><td><code>data.transaction.maxPriorityFeePerGas</code></td><td>string</td><td>EIP-1559 priority fee per gas, on type <code>0x2</code> and later.</td></tr>
<tr><td><code>data.transaction.type</code></td><td>string</td><td>EIP-2718 transaction type, hex-encoded.</td></tr>
<tr><td><code>data.transaction.chainId</code></td><td>string</td><td>Chain ID, hex-encoded.</td></tr>
<tr><td><code>data.transaction.accessList</code></td><td>array</td><td>EIP-2930 access list, on type <code>0x1</code> and later.</td></tr>
<tr><td><code>data.transaction.v, r, s, yParity</code></td><td>string</td><td>Signature values.</td></tr>
</tbody></table>

Inclusion is not execution success: a mined transaction can still have reverted. To learn the outcome, use [`transactionReceipts`](transactionreceipts-evm-data-stream.md).

## Use Cases

* Confirming that a broadcast transaction was included in a block
* Wallet activity feeds for a set of up to 50 addresses
* Watching every call to a contract (`{ "to": contract }`)
* Pairing with [`newPendingTransactions`](newpendingtransactions-evm-data-stream.md) to measure mempool-to-block latency

## Error Handling

<table data-search="false"><thead><tr><th>Code</th><th>Message</th><th>Cause</th></tr></thead><tbody>
<tr><td><code>-32602</code></td><td><code>addresses[N] requires from or to</code></td><td>An <code>addresses</code> rule is empty.</td></tr>
<tr><td><code>-32602</code></td><td><code>invalid addresses[N].to: invalid EVM address "…"</code></td><td>An address in a rule is not a 20-byte hex address.</td></tr>
<tr><td><code>-32602</code></td><td><code>invalid newMinedTransactions request: json: unknown field "filters"</code></td><td>The fields were nested in <code>filters</code>. Put <code>hashesOnly</code> and <code>addresses</code> at the top level.</td></tr>
<tr><td><code>-32602</code></td><td><code>invalid newMinedTransactions request: json: unknown field "…"</code></td><td>The request has a field this topic does not accept.</td></tr>
<tr><td><code>-32001</code></td><td><code>subscription limit exceeded</code></td><td>You already have 15 active or pending subscriptions.</td></tr>
</tbody></table>
