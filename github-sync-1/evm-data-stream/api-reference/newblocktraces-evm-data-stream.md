---
description: >-
  Stream each new block's matching transaction call traces in one message. Complete guide on how to use the newBlockTraces topic in GetBlock EVM Data Stream documentation.
---

# newBlockTraces - EVM Data Stream

The `newBlockTraces` topic sends **one notification per block**, carrying the block's identity and an array of call traces for the transactions that match your filters. It carries the same trace data as [`newMinedTransactionsTraces`](newminedtransactionstraces-evm-data-stream.md), grouped by block.

{% hint style="warning" %}
**WebSocket-only topic.** Subscribe with [`getblock_subscribe`](getblock_subscribe-evm-data-stream.md) on any [network endpoint](./#endpoints).
{% endhint %}

{% hint style="info" %}
Without filters, every notification carries the trace of every transaction in the block, which can be several megabytes on Ethereum. Filter by address or call type unless you are building a full trace index.
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
{"jsonrpc":"2.0","id":1,"method":"getblock_subscribe","params":[{"topic":"newBlockTraces","filters":{"toAddress":"0xdAC17F958D2ee523a2206206994597C13D831ec7"}}]}
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
      topic: 'newBlockTraces',
      filters: {
        toAddress: '0xdAC17F958D2ee523a2206206994597C13D831ec7'
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
  console.log(removed ? 'REVERTED' : 'block', parseInt(data.blockNumber, 16), `${data.traces.length} matching traces`);
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
                "topic": "newBlockTraces",
                "filters": {
                    "toAddress": "0xdAC17F958D2ee523a2206206994597C13D831ec7"
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
            print('REVERTED' if removed else 'block', int(data['blockNumber'], 16), len(data['traces']), 'matching traces')

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
  "result": "0x23053ab0cfd75fdbae902fbb73dd1ae9"
}
```

Every later message is an event notification (traces shortened to one entry):

```json
{
  "jsonrpc": "2.0",
  "method": "getblock_subscribe",
  "params": {
    "subscription": "0x23053ab0cfd75fdbae902fbb73dd1ae9",
    "result": {
      "eventId": "eth:mainnet:traces:0x19ddc6864bd5ec1cdbd493e3d3653b529d79293a67735…",
      "removed": false,
      "revision": 116876400,
      "data": {
        "blockHash": "0x19ddc6864bd5ec1cdbd493e3d3653b529d79293a6773517018153cdff6ac0be2",
        "blockNumber": "0x18d1e1e",
        "traces": [
          {
            "txHash": "0x6b362543c83c1d18b27701235338c05a7e04c2e5098bed46d60afe9a0f17d2a1",
            "result": {
              "from": "0x4b3dd3690ba06b2a73089e02381d0eba9a7dbca5",
              "gas": "0xb435",
              "gasUsed": "0xa15d",
              "to": "0xdac17f958d2ee523a2206206994597c13d831ec7",
              "input": "0xa9059cbb000000000000000000000000a2050435e59f41300b7787808e976255faa198e50000000000000000000000000000000000000000000000000000000006f4b360",
              "value": "0x0",
              "type": "CALL"
            }
          },
          "…"
        ]
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
<tr><td><code>data.blockHash</code></td><td>string</td><td>Block hash.</td></tr>
<tr><td><code>data.blockNumber</code></td><td>string</td><td>Block number, hex-encoded.</td></tr>
<tr><td><code>data.traces</code></td><td>array</td><td>Traces of the matching transactions, in block order.</td></tr>
<tr><td><code>data.traces[].txHash</code></td><td>string</td><td>Transaction hash.</td></tr>
<tr><td><code>data.traces[].result</code></td><td>object</td><td>Top-level call frame. Its fields are below.</td></tr>
<tr><td><code>data.traces[].result.type</code></td><td>string</td><td>Frame type: <code>CALL</code>, <code>DELEGATECALL</code>, <code>STATICCALL</code>, <code>CREATE</code>, <code>CREATE2</code>, or <code>SELFDESTRUCT</code>.</td></tr>
<tr><td><code>data.traces[].result.from</code></td><td>string</td><td>Caller address.</td></tr>
<tr><td><code>data.traces[].result.to</code></td><td>string</td><td>Callee address, or the created contract.</td></tr>
<tr><td><code>data.traces[].result.value</code></td><td>string</td><td>Native value sent with the call in wei, hex-encoded.</td></tr>
<tr><td><code>data.traces[].result.gas</code></td><td>string</td><td>Gas available to the frame, hex-encoded.</td></tr>
<tr><td><code>data.traces[].result.gasUsed</code></td><td>string</td><td>Gas used by the frame, hex-encoded.</td></tr>
<tr><td><code>data.traces[].result.input</code></td><td>string</td><td>Call data.</td></tr>
<tr><td><code>data.traces[].result.output</code></td><td>string</td><td>Return data, when there is any.</td></tr>
<tr><td><code>data.traces[].result.error</code></td><td>string</td><td>Error message when the frame failed, for example <code>execution reverted</code>.</td></tr>
<tr><td><code>data.traces[].result.revertReason</code></td><td>string</td><td>Decoded revert reason, when available.</td></tr>
<tr><td><code>data.traces[].result.calls</code></td><td>array</td><td>Child frames, each with the same fields, recursively.</td></tr>
</tbody></table>

{% hint style="info" %}
Traces use the node's `callTracer` format (as `debug_traceTransaction` returns it), so fields can vary by network client. For example, Robinhood Mainnet frames also include `beforeEVMTransfers` and `afterEVMTransfers`. Trace availability also depends on the network.
{% endhint %}

## Use Cases

* Building a trace or internal-transaction index block by block
* Per-block analytics of contract interactions
* Reconciling all internal transfers of a block in one atomic update

## Error Handling

<table data-search="false"><thead><tr><th>Code</th><th>Message</th><th>Cause</th></tr></thead><tbody>
<tr><td><code>-32602</code></td><td><code>filters.status must be success or failed</code></td><td><code>status</code> has an unsupported value.</td></tr>
<tr><td><code>-32602</code></td><td><code>unsupported filters.callType "…"</code></td><td><code>callType</code> has an unsupported value.</td></tr>
<tr><td><code>-32602</code></td><td><code>invalid filters.toAddress address "…"</code></td><td>An address filter is not a 20-byte hex address. The same applies to <code>fromAddress</code>.</td></tr>
<tr><td><code>-32602</code></td><td><code>invalid newBlockTraces request: json: unknown field "…"</code></td><td>The request has a field this topic does not accept.</td></tr>
<tr><td><code>-32001</code></td><td><code>subscription limit exceeded</code></td><td>You already have 15 active or pending subscriptions.</td></tr>
</tbody></table>
