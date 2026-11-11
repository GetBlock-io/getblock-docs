---
description: >-
  Stream raw EVM transaction receipts filtered by status, sender, recipient, and type. Complete guide on how to use the transactionReceipts topic in GetBlock EVM Data Stream documentation.
---

# transactionReceipts - EVM Data Stream

The `transactionReceipts` topic sends one notification per matching mined transaction, carrying its **original receipt**, exactly as `eth_getTransactionReceipt` returns it. Use it to learn whether transactions succeeded, what they cost, which contracts they created, and which logs they emitted.

{% hint style="warning" %}
**WebSocket-only topic.** Subscribe with [`getblock_subscribe`](getblock_subscribe-evm-data-stream.md) on any [network endpoint](./#endpoints).
{% endhint %}

## Parameters

<table data-search="false"><thead><tr><th>Parameter</th><th>Type</th><th>Required</th><th>Description</th></tr></thead><tbody>
<tr><td><code>filters.status</code></td><td>string</td><td>No</td><td><code>success</code> or <code>failure</code>.</td></tr>
<tr><td><code>filters.from</code></td><td>string</td><td>No</td><td>Transaction sender.</td></tr>
<tr><td><code>filters.to</code></td><td>string</td><td>No</td><td>Transaction recipient.</td></tr>
<tr><td><code>filters.contractCreated</code></td><td>boolean</td><td>No</td><td><code>true</code> for contract-creation receipts only; <code>false</code> for ordinary transactions only.</td></tr>
<tr><td><code>filters.transactionType</code></td><td>string</td><td>No</td><td>EIP-2718 transaction type as a hex quantity: <code>"0x0"</code> legacy, <code>"0x1"</code> access list, <code>"0x2"</code> EIP-1559, <code>"0x3"</code> blob, <code>"0x4"</code> EIP-7702.</td></tr>
</tbody></table>

## Request Example

{% tabs %}
{% tab title="wscat" %}
{% code overflow="wrap" %}
```bash
# WebSocket-only. Connect first:
wscat -c 'wss://stream.eu-central-1.getblock.io/v1/eth-mainnet/stream' -H 'Authorization: Bearer <API-KEY>'

# Then send:
{"jsonrpc":"2.0","id":1,"method":"getblock_subscribe","params":[{"topic":"transactionReceipts","filters":{"from":"0x965e7b3578110a781b86b1cca9b2d7824b62a678","status":"failure"}}]}
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
      topic: 'transactionReceipts',
      filters: {
        from: '0x965e7b3578110a781b86b1cca9b2d7824b62a678',
        status: 'failure'
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
  const ok = data.status === '0x1';
  console.log(removed ? 'REVERTED' : ok ? 'success' : 'FAILED', data.transactionHash, 'gas', parseInt(data.gasUsed, 16));
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
                "topic": "transactionReceipts",
                "filters": {
                    "from": "0x965e7b3578110a781b86b1cca9b2d7824b62a678",
                    "status": "failure"
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
            ok = data['status'] == '0x1'
            print('REVERTED' if removed else 'success' if ok else 'FAILED', data['transactionHash'], 'gas', int(data['gasUsed'], 16))

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
  "result": "0x9247e050d6cf2adba09143092ee169a9"
}
```

Every later message is an event notification (a failed transaction):

```json
{
  "jsonrpc": "2.0",
  "method": "getblock_subscribe",
  "params": {
    "subscription": "0x9247e050d6cf2adba09143092ee169a9",
    "result": {
      "eventId": "eth:mainnet:receipts:0x19ddc6864bd5ec1cdbd493e3d3653b529d79293a677…",
      "removed": false,
      "revision": 116876401,
      "data": {
        "blockHash": "0x19ddc6864bd5ec1cdbd493e3d3653b529d79293a6773517018153cdff6ac0be2",
        "blockNumber": "0x18d1e1e",
        "contractAddress": null,
        "cumulativeGasUsed": "0x52a2e2",
        "effectiveGasPrice": "0x19a412a0",
        "from": "0x965e7b3578110a781b86b1cca9b2d7824b62a678",
        "gasUsed": "0x7aa8",
        "logs": [],
        "logsBloom": "0x0000000000000000000000000000000000000000000000000000000000000000…",
        "status": "0x0",
        "to": "0xe84ed58fe56169acca155867ab186a30a4a2e731",
        "transactionHash": "0xa5e02c3cdc33428d01de9cf5acc0c43795b5f34ac34d918d07a363e8d45a6bdc",
        "transactionIndex": "0x5a",
        "type": "0x2"
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
<tr><td><code>data</code></td><td>object</td><td>The receipt, in the same shape as <code>eth_getTransactionReceipt</code>.</td></tr>
<tr><td><code>data.transactionHash</code></td><td>string</td><td>Transaction hash.</td></tr>
<tr><td><code>data.transactionIndex</code></td><td>string</td><td>Transaction's position in the block, hex-encoded.</td></tr>
<tr><td><code>data.blockHash</code></td><td>string</td><td>Block hash.</td></tr>
<tr><td><code>data.blockNumber</code></td><td>string</td><td>Block number, hex-encoded.</td></tr>
<tr><td><code>data.from</code></td><td>string</td><td>Sender.</td></tr>
<tr><td><code>data.to</code></td><td>string</td><td>Recipient. <code>null</code> for contract creation.</td></tr>
<tr><td><code>data.contractAddress</code></td><td>string</td><td>Address of the created contract, or <code>null</code>.</td></tr>
<tr><td><code>data.status</code></td><td>string</td><td><code>0x1</code> for success, <code>0x0</code> for failure (reverted).</td></tr>
<tr><td><code>data.type</code></td><td>string</td><td>EIP-2718 transaction type, hex-encoded.</td></tr>
<tr><td><code>data.gasUsed</code></td><td>string</td><td>Gas used by this transaction, hex-encoded.</td></tr>
<tr><td><code>data.cumulativeGasUsed</code></td><td>string</td><td>Gas used in the block up to and including this transaction, hex-encoded.</td></tr>
<tr><td><code>data.effectiveGasPrice</code></td><td>string</td><td>Price paid per gas in wei, hex-encoded. Fee = <code>gasUsed × effectiveGasPrice</code>.</td></tr>
<tr><td><code>data.logs</code></td><td>array</td><td>Logs emitted by the transaction, each in the [<code>logs</code>](logs-evm-data-stream.md#response-parameters) shape.</td></tr>
<tr><td><code>data.logsBloom</code></td><td>string</td><td>Bloom filter for the receipt's logs.</td></tr>
</tbody></table>

Network-specific receipt fields, such as `blobGasUsed` on blob transactions or L1 fee fields on rollups, are passed through unchanged.

## Use Cases

* Confirming that a submitted transaction succeeded or failed, without polling
* Alerting on failed transactions from your hot wallets
* Detecting new contract deployments (`contractCreated: true`)
* Gas-cost accounting per transaction

## Error Handling

<table data-search="false"><thead><tr><th>Code</th><th>Message</th><th>Cause</th></tr></thead><tbody>
<tr><td><code>-32602</code></td><td><code>invalid status "…": must be success or failure</code></td><td><code>status</code> has an unsupported value. Note it is <code>failure</code> here, but <code>failed</code> on the trace topics.</td></tr>
<tr><td><code>-32602</code></td><td><code>… cannot unmarshal hex string without 0x prefix into Go struct field .filters.transactionType …</code></td><td><code>transactionType</code> was sent as a decimal string. Use <code>"0x2"</code>, not <code>"2"</code>.</td></tr>
<tr><td><code>-32602</code></td><td><code>invalid from: invalid EVM address "…"</code></td><td><code>from</code> is not a 20-byte hex address.</td></tr>
<tr><td><code>-32602</code></td><td><code>invalid to: invalid EVM address "…"</code></td><td><code>to</code> is not a 20-byte hex address.</td></tr>
<tr><td><code>-32602</code></td><td><code>invalid transactionReceipts request: json: unknown field "…"</code></td><td>The request has a field this topic does not accept.</td></tr>
<tr><td><code>-32001</code></td><td><code>subscription limit exceeded</code></td><td>You already have 15 active or pending subscriptions.</td></tr>
</tbody></table>
