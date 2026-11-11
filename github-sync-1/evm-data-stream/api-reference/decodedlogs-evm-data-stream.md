---
description: >-
  Stream EVM event logs decoded with your own ABI into named, typed arguments. Complete guide on how to use the decodedLogs topic in GetBlock EVM Data Stream documentation.
---

# decodedLogs - EVM Data Stream

The `decodedLogs` topic takes the event ABI you provide, matches logs by those events' signatures, and sends each match **already decoded**: the event name, its signature, and every argument with its name, type, and value. You don't need an ABI library on the client.

{% hint style="warning" %}
**WebSocket-only topic.** Subscribe with [`getblock_subscribe`](getblock_subscribe-evm-data-stream.md) on any [network endpoint](./#endpoints).
{% endhint %}

## Parameters

<table data-search="false"><thead><tr><th>Parameter</th><th>Type</th><th>Required</th><th>Description</th></tr></thead><tbody>
<tr><td><code>abi</code></td><td>array</td><td>Yes</td><td>1–16 event ABI entries. Each needs <code>"type": "event"</code>, a <code>name</code>, and <code>inputs</code>. Function, error, and anonymous event entries are rejected.</td></tr>
<tr><td><code>filters.address</code></td><td>string or array</td><td>No</td><td>Emitting contract address, or an array of addresses to match any of. Omit to decode matching events from every contract.</td></tr>
</tbody></table>

`abi` sits at the top level of the request object, next to `topic`, not inside `filters`. Each `inputs` entry takes `name`, `type`, and `indexed`, exactly as in a Solidity JSON ABI. You can paste event entries straight from a contract's ABI file.

## Request Example

{% tabs %}
{% tab title="wscat" %}
{% code overflow="wrap" %}
```bash
# WebSocket-only. Connect first:
wscat -c 'wss://stream.eu-central-1.getblock.io/v1/eth-mainnet/stream' -H 'Authorization: Bearer <API-KEY>'

# Then send:
{"jsonrpc":"2.0","id":1,"method":"getblock_subscribe","params":[{"topic":"decodedLogs","abi":[{"type":"event","name":"Transfer","inputs":[{"name":"from","type":"address","indexed":true},{"name":"to","type":"address","indexed":true},{"name":"value","type":"uint256","indexed":false}]}],"filters":{"address":"0xdAC17F958D2ee523a2206206994597C13D831ec7"}}]}
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
      topic: 'decodedLogs',
      abi: [
        {
          type: 'event',
          name: 'Transfer',
          inputs: [
            {
              name: 'from',
              type: 'address',
              indexed: true
            },
            {
              name: 'to',
              type: 'address',
              indexed: true
            },
            {
              name: 'value',
              type: 'uint256',
              indexed: false
            }
          ]
        }
      ],
      filters: {
        address: '0xdAC17F958D2ee523a2206206994597C13D831ec7'
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
  const args = Object.fromEntries(data.arguments.map((a) => [a.name, a.value]));
  console.log(removed ? 'REMOVED' : data.event, args);
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
                "topic": "decodedLogs",
                "abi": [
                    {
                        "type": "event",
                        "name": "Transfer",
                        "inputs": [
                            {
                                "name": "from",
                                "type": "address",
                                "indexed": True
                            },
                            {
                                "name": "to",
                                "type": "address",
                                "indexed": True
                            },
                            {
                                "name": "value",
                                "type": "uint256",
                                "indexed": False
                            }
                        ]
                    }
                ],
                "filters": {
                    "address": "0xdAC17F958D2ee523a2206206994597C13D831ec7"
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
            args = {a['name']: a['value'] for a in data['arguments']}
            print('REMOVED' if removed else data['event'], args)

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
  "result": "0x1646a726cfe2590f965eae6aa42a9745"
}
```

Every later message is an event notification:

```json
{
  "jsonrpc": "2.0",
  "method": "getblock_subscribe",
  "params": {
    "subscription": "0x1646a726cfe2590f965eae6aa42a9745",
    "result": {
      "eventId": "eth:mainnet:receipts:0x19ddc6864bd5ec1cdbd493e3d3653b529d79293a677…",
      "removed": false,
      "revision": 116876401,
      "data": {
        "event": "Transfer",
        "signature": "Transfer(address,address,uint256)",
        "arguments": [
          {
            "name": "from",
            "type": "address",
            "indexed": true,
            "value": "0x4b3dD3690ba06b2A73089E02381d0EBA9a7DBCA5"
          },
          {
            "name": "to",
            "type": "address",
            "indexed": true,
            "value": "0xa2050435E59f41300B7787808E976255fAA198e5"
          },
          {
            "name": "value",
            "type": "uint256",
            "indexed": false,
            "value": "0x6f4b360"
          }
        ],
        "address": "0xdac17f958d2ee523a2206206994597c13d831ec7",
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
<tr><td><code>data.event</code></td><td>string</td><td>Event name from your ABI.</td></tr>
<tr><td><code>data.signature</code></td><td>string</td><td>Canonical event signature, for example <code>Transfer(address,address,uint256)</code>.</td></tr>
<tr><td><code>data.arguments</code></td><td>array</td><td>Decoded arguments, in ABI order.</td></tr>
<tr><td><code>data.arguments[].name</code></td><td>string</td><td>Argument name from your ABI.</td></tr>
<tr><td><code>data.arguments[].type</code></td><td>string</td><td>Solidity type from your ABI.</td></tr>
<tr><td><code>data.arguments[].indexed</code></td><td>boolean</td><td>Whether the argument was an indexed topic.</td></tr>
<tr><td><code>data.arguments[].value</code></td><td>string</td><td>Decoded value. Integers are hex quantities; addresses keep EIP-55 checksum casing.</td></tr>
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
Matching is by event signature, so a log from any contract that emits the same signature is decoded unless you also set `filters.address`. ERC-20 and ERC-721 both emit `Transfer(address,address,uint256)`, with different arguments indexed, so an unfiltered subscription can fail to decode, or mis-decode, logs from the other standard. For token transfers, prefer [`erc20Transfers`](erc20transfers-evm-data-stream.md) or [`nftTransfers`](nfttransfers-evm-data-stream.md).
{% endhint %}

## Use Cases

* Consuming DeFi events such as `Swap`, `Deposit`, or `Borrow` without an ABI library on the client
* Low-code and serverless consumers that want JSON arguments directly
* Watching up to 16 events of one protocol on a single subscription
* Feeding decoded events straight into analytics tables

## Error Handling

<table data-search="false"><thead><tr><th>Code</th><th>Message</th><th>Cause</th></tr></thead><tbody>
<tr><td><code>-32602</code></td><td><code>invalid decodedLogs abi: must contain between 1 and 16 events</code></td><td><code>abi</code> is missing, empty, or has more than 16 entries.</td></tr>
<tr><td><code>-32602</code></td><td><code>invalid decodedLogs abi entry N: type must be event, got "function"</code></td><td>An ABI entry is not an event.</td></tr>
<tr><td><code>-32602</code></td><td><code>invalid decodedLogs abi entry N: anonymous events are unsupported</code></td><td>An ABI entry has <code>"anonymous": true</code>. Anonymous events have no signature topic to match on.</td></tr>
<tr><td><code>-32602</code></td><td><code>invalid decodedLogs address filter "…"</code></td><td><code>filters.address</code> is not a 20-byte hex address.</td></tr>
<tr><td><code>-32602</code></td><td><code>invalid decodedLogs request: json: unknown field "…"</code></td><td>The request has a field this topic does not accept.</td></tr>
<tr><td><code>-32001</code></td><td><code>subscription limit exceeded</code></td><td>You already have 15 active or pending subscriptions.</td></tr>
</tbody></table>
