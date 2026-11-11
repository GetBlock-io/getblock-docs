---
description: >-
  Stream the call trace of each matching mined transaction, including internal calls. Complete guide on how to use the newMinedTransactionsTraces topic in GetBlock EVM Data Stream documentation.
---

# newMinedTransactionsTraces - EVM Data Stream

The `newMinedTransactionsTraces` topic sends **one notification per matching mined transaction**, carrying its full call trace: the top-level call and every internal call, delegate call, and contract creation beneath it. Use it to see value and control flow that logs don't show, such as internal ETH transfers.

{% hint style="warning" %}
**WebSocket-only topic.** Subscribe with [`getblock_subscribe`](getblock_subscribe-evm-data-stream.md) on any [network endpoint](./#endpoints).
{% endhint %}

## Parameters

<table data-search="false"><thead><tr><th>Parameter</th><th>Type</th><th>Required</th><th>Description</th></tr></thead><tbody>
<tr><td><code>filters.transactionHash</code></td><td>string or array</td><td>No</td><td>One transaction hash, or an array of hashes.</td></tr>
<tr><td><code>filters.fromAddress</code></td><td>string or array</td><td>No</td><td>Match traces with any call frame <strong>from</strong> these addresses.</td></tr>
<tr><td><code>filters.toAddress</code></td><td>string or array</td><td>No</td><td>Match traces with any call frame <strong>to</strong> these addresses.</td></tr>
<tr><td><code>filters.callType</code></td><td>string or array</td><td>No</td><td>Match frames of these types: <code>call</code>, <code>delegatecall</code>, <code>staticcall</code>, <code>create</code>, or <code>create2</code>.</td></tr>
<tr><td><code>filters.status</code></td><td>string</td><td>No</td><td><code>success</code> or <code>failed</code>: match traces that contain a frame with this status.</td></tr>
</tbody></table>

Filters match on **any** frame in the call tree, not only the top-level call. A transaction that touches a contract through an internal call matches `toAddress` for that contract, and `status: "failed"` matches a transaction with a failed internal call even when its top-level call succeeded. Check `error` on each frame to see which call failed.

## Request Example

{% tabs %}
{% tab title="wscat" %}
{% code overflow="wrap" %}
```bash
# WebSocket-only. Connect first:
wscat -c 'wss://stream.eu-central-1.getblock.io/v1/eth-mainnet/stream' -H 'Authorization: Bearer <API-KEY>'

# Then send:
{"jsonrpc":"2.0","id":1,"method":"getblock_subscribe","params":[{"topic":"newMinedTransactionsTraces","filters":{"toAddress":"0xdAC17F958D2ee523a2206206994597C13D831ec7","status":"failed"}}]}
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
      topic: 'newMinedTransactionsTraces',
      filters: {
        toAddress: '0xdAC17F958D2ee523a2206206994597C13D831ec7',
        status: 'failed'
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
  // Collect every failed frame in the call tree
  const failed = [];
  const walk = (frame) => { if (frame.error) failed.push(frame); (frame.calls ?? []).forEach(walk); };
  walk(data.trace.result);
  console.log(removed ? 'REVERTED' : 'trace', data.trace.txHash, failed.map((f) => `${f.type} ${f.to}: ${f.error}`));
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
                "topic": "newMinedTransactionsTraces",
                "filters": {
                    "toAddress": "0xdAC17F958D2ee523a2206206994597C13D831ec7",
                    "status": "failed"
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
            # Collect every failed frame in the call tree
            failed, stack = [], [data['trace']['result']]
            while stack:
                frame = stack.pop()
                if 'error' in frame:
                    failed.append(f"{frame['type']} {frame.get('to')}: {frame['error']}")
                stack.extend(frame.get('calls', []))
            print('REVERTED' if removed else 'trace', data['trace']['txHash'], failed)

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
  "result": "0x36e43d4421db2f9485b82bd1c7b1e35a"
}
```

Every later message is an event notification (a failed transaction):

```json
{
  "jsonrpc": "2.0",
  "method": "getblock_subscribe",
  "params": {
    "subscription": "0x36e43d4421db2f9485b82bd1c7b1e35a",
    "result": {
      "eventId": "eth:mainnet:traces:0x6f38a91f1c46e7b558e0560eb9ad23341dbbf607929bc…",
      "removed": false,
      "revision": 117086349,
      "data": {
        "removed": false,
        "blockHash": "0x6f38a91f1c46e7b558e0560eb9ad23341dbbf607929bc1e1aa6a0fc54c58fbb5",
        "blockNumber": "0x18d1ec4",
        "trace": {
          "txHash": "0xaccdf1518ad74a7e3061cadd17e96110f84299f2e2bf3f96074acdb9c263c640",
          "result": {
            "from": "0x837df54cdcb99df1518c5bf6f632e0d97c4c7aa6",
            "gas": "0x493e0",
            "gasUsed": "0xb3a1",
            "to": "0xe3e7e7e4ccf993fb41b2a86d240bdadfefae6e66",
            "input": "0xb63d5113000000000000000000000000dac17f958d2ee523a2206206994597c1…",
            "output": "0x06427aeb000000000000000000000000000000000000000000000000000000000000000500000000000000000000000000000000000000000000000000000000000001e7",
            "error": "execution reverted",
            "value": "0x0",
            "type": "CALL"
          }
        }
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
<tr><td><code>data.removed</code></td><td>boolean</td><td>Mirrors the envelope's <code>removed</code>.</td></tr>
<tr><td><code>data.blockHash</code></td><td>string</td><td>Hash of the block that contains the transaction.</td></tr>
<tr><td><code>data.blockNumber</code></td><td>string</td><td>Block number, hex-encoded.</td></tr>
<tr><td><code>data.trace.txHash</code></td><td>string</td><td>Transaction hash.</td></tr>
<tr><td><code>data.trace.result</code></td><td>object</td><td>Top-level call frame. Its fields are below.</td></tr>
<tr><td><code>data.trace.result.type</code></td><td>string</td><td>Frame type: <code>CALL</code>, <code>DELEGATECALL</code>, <code>STATICCALL</code>, <code>CREATE</code>, <code>CREATE2</code>, or <code>SELFDESTRUCT</code>.</td></tr>
<tr><td><code>data.trace.result.from</code></td><td>string</td><td>Caller address.</td></tr>
<tr><td><code>data.trace.result.to</code></td><td>string</td><td>Callee address, or the created contract.</td></tr>
<tr><td><code>data.trace.result.value</code></td><td>string</td><td>Native value sent with the call in wei, hex-encoded.</td></tr>
<tr><td><code>data.trace.result.gas</code></td><td>string</td><td>Gas available to the frame, hex-encoded.</td></tr>
<tr><td><code>data.trace.result.gasUsed</code></td><td>string</td><td>Gas used by the frame, hex-encoded.</td></tr>
<tr><td><code>data.trace.result.input</code></td><td>string</td><td>Call data.</td></tr>
<tr><td><code>data.trace.result.output</code></td><td>string</td><td>Return data, when there is any.</td></tr>
<tr><td><code>data.trace.result.error</code></td><td>string</td><td>Error message when the frame failed, for example <code>execution reverted</code>.</td></tr>
<tr><td><code>data.trace.result.revertReason</code></td><td>string</td><td>Decoded revert reason, when available.</td></tr>
<tr><td><code>data.trace.result.calls</code></td><td>array</td><td>Child frames, each with the same fields, recursively.</td></tr>
</tbody></table>

{% hint style="info" %}
Traces use the node's `callTracer` format (as `debug_traceTransaction` returns it), so fields can vary by network client. For example, Robinhood Mainnet frames also include `beforeEVMTransfers` and `afterEVMTransfers`. Trace availability also depends on the network.
{% endhint %}

## Use Cases

* Detecting internal native-token transfers into your addresses
* Debugging and alerting on failed transactions with their revert point
* Analysing contract-to-contract interactions of a protocol
* Security monitoring for unexpected `delegatecall`s or contract creations

## Error Handling

<table data-search="false"><thead><tr><th>Code</th><th>Message</th><th>Cause</th></tr></thead><tbody>
<tr><td><code>-32602</code></td><td><code>filters.status must be success or failed</code></td><td><code>status</code> has an unsupported value. Note it is <code>failed</code> here, but <code>failure</code> on <code>transactionReceipts</code>.</td></tr>
<tr><td><code>-32602</code></td><td><code>unsupported filters.callType "…"</code></td><td><code>callType</code> has an unsupported value.</td></tr>
<tr><td><code>-32602</code></td><td><code>invalid filters.toAddress address "…"</code></td><td>An address filter is not a 20-byte hex address. The same applies to <code>fromAddress</code>.</td></tr>
<tr><td><code>-32602</code></td><td><code>invalid newMinedTransactionsTraces request: json: unknown field "…"</code></td><td>The request has a field this topic does not accept.</td></tr>
<tr><td><code>-32001</code></td><td><code>subscription limit exceeded</code></td><td>You already have 15 active or pending subscriptions.</td></tr>
</tbody></table>
