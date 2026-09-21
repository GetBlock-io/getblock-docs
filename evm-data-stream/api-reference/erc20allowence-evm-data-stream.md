---
description: >-
  Stream decoded ERC-20 Approval (allowance) events filtered by token, owner, and spender. Complete guide on how to use the erc20Allowence topic in GetBlock EVM Data Stream documentation.
---

# erc20Allowence - EVM Data Stream

The `erc20Allowence` topic sends one notification per ERC-20 `Approval` event: an owner granting, changing, or revoking a spender's allowance. Use it for approval alerts, risk monitoring, and allowance dashboards.

{% hint style="warning" %}
**WebSocket-only topic.** Subscribe with [`getblock_subscribe`](getblock_subscribe-evm-data-stream.md) on any [network endpoint](./#endpoints).
{% endhint %}

{% hint style="danger" %}
`erc20Allowence` is the current public topic name, including the misspelling. Send it exactly as shown.
{% endhint %}

## Parameters

<table data-search="false"><thead><tr><th>Parameter</th><th>Type</th><th>Required</th><th>Description</th></tr></thead><tbody>
<tr><td><code>filters.tokenContract</code></td><td>string</td><td>No</td><td>Token contract address.</td></tr>
<tr><td><code>filters.from</code></td><td>string</td><td>No</td><td>Allowance <strong>owner</strong>: the address granting the approval.</td></tr>
<tr><td><code>filters.to</code></td><td>string</td><td>No</td><td>Allowance <strong>spender</strong>: the address being approved.</td></tr>
</tbody></table>

The filter names `from` and `to` are reused from `erc20Transfers`: `from` is the owner and `to` is the spender.

## Request Example

{% tabs %}
{% tab title="wscat" %}
{% code overflow="wrap" %}
```bash
# WebSocket-only. Connect first:
wscat -c 'wss://stream.eu-central-1.getblock.io/v1/eth-mainnet/stream' -H 'Authorization: Bearer <API-KEY>'

# Then send:
{"jsonrpc":"2.0","id":1,"method":"getblock_subscribe","params":[{"topic":"erc20Allowence","filters":{"from":"0x709c9dffb1e7e37aa62950048328d5c17354a7aa"}}]}
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
      topic: 'erc20Allowence',
      filters: {
        from: '0x709c9dffb1e7e37aa62950048328d5c17354a7aa'
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
  const revoked = BigInt(data.value) === 0n;
  console.log(removed ? 'REVERTED' : revoked ? 'revoked' : 'approved', data.tokenContract, 'spender', data.to, data.value);
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
                "topic": "erc20Allowence",
                "filters": {
                    "from": "0x709c9dffb1e7e37aa62950048328d5c17354a7aa"
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
            revoked = int(data['value'], 16) == 0
            print('REVERTED' if removed else 'revoked' if revoked else 'approved', data['tokenContract'], 'spender', data['to'], data['value'])

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
        "tokenContract": "0xa0b86991c6218b36c1d19d4a2e9eb0ce3606eb48",
        "from": "0x709c9dffb1e7e37aa62950048328d5c17354a7aa",
        "to": "0xe09ad398e1deee1f18fec1255c58d3dff57068a3",
        "value": "0x3938700",
        "blockNumber": "0x18d1e1e",
        "transactionHash": "0x8a31cb937fb624bae016a3c73389f4596e82150a159b017d54ad3e18c039cc4f",
        "transactionIndex": "0x7",
        "blockHash": "0x19ddc6864bd5ec1cdbd493e3d3653b529d79293a6773517018153cdff6ac0be2",
        "blockTimestamp": "0x6ab110c3",
        "logIndex": "0x14",
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
<tr><td><code>data.from</code></td><td>string</td><td>Owner: the address that granted the allowance.</td></tr>
<tr><td><code>data.to</code></td><td>string</td><td>Spender: the address allowed to spend.</td></tr>
<tr><td><code>data.value</code></td><td>string</td><td>New allowance in the token's smallest unit, hex-encoded. <code>0x0</code> is a revocation; <code>0xff…ff</code> (2^256 − 1) is an unlimited approval.</td></tr>
<tr><td><code>data.address</code></td><td>string</td><td>Address of the contract that emitted the log.</td></tr>
<tr><td><code>data.blockNumber</code></td><td>string</td><td>Block number, hex-encoded.</td></tr>
<tr><td><code>data.blockHash</code></td><td>string</td><td>Hash of the block that contains the log.</td></tr>
<tr><td><code>data.blockTimestamp</code></td><td>string</td><td>Block timestamp in Unix seconds, hex-encoded.</td></tr>
<tr><td><code>data.transactionHash</code></td><td>string</td><td>Hash of the transaction that emitted the log.</td></tr>
<tr><td><code>data.transactionIndex</code></td><td>string</td><td>Transaction's position in the block, hex-encoded.</td></tr>
<tr><td><code>data.logIndex</code></td><td>string</td><td>Log's position in the block, hex-encoded.</td></tr>
<tr><td><code>data.removed</code></td><td>boolean</td><td>The underlying log's <code>removed</code> flag. Use the envelope's <code>removed</code> for reorg handling.</td></tr>
</tbody></table>

`value` is the new allowance, not an increment. Tokens that support `permit` (EIP-2612) also emit `Approval`, so gasless approvals appear here too.

## Use Cases

* Alerting users when their wallet grants an unlimited approval
* Flagging approvals to known malicious spenders
* Keeping an allowance dashboard or revoke tool up to date
* Detecting that a user approved your contract so your UI can move to the next step

## Error Handling

<table data-search="false"><thead><tr><th>Code</th><th>Message</th><th>Cause</th></tr></thead><tbody>
<tr><td><code>-32602</code></td><td><code>invalid tokenContract: invalid EVM address "…"</code></td><td><code>tokenContract</code> is not a 20-byte hex address.</td></tr>
<tr><td><code>-32602</code></td><td><code>invalid from: invalid EVM address "…"</code></td><td><code>from</code> is not a 20-byte hex address.</td></tr>
<tr><td><code>-32602</code></td><td><code>invalid to: invalid EVM address "…"</code></td><td><code>to</code> is not a 20-byte hex address.</td></tr>
<tr><td><code>-32602</code></td><td><code>invalid erc20Allowence request: json: unknown field "…"</code></td><td>The request has a field this topic does not accept.</td></tr>
<tr><td><code>-32001</code></td><td><code>subscription limit exceeded</code></td><td>You already have 15 active or pending subscriptions.</td></tr>
</tbody></table>
