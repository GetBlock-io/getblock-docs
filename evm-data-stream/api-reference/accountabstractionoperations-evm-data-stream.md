---
description: >-
  Stream decoded ERC-4337 EntryPoint events: UserOperation results, account deployments, and revert reasons. Complete guide on how to use the accountAbstractionOperations topic in GetBlock EVM Data Stream documentation.
---

# accountAbstractionOperations - EVM Data Stream

The `accountAbstractionOperations` topic decodes the events of the canonical [ERC-4337](https://eips.ethereum.org/EIPS/eip-4337) EntryPoint contracts. It tells you when a smart account's UserOperation executed and whether it succeeded, when a new smart account was deployed, and why an operation reverted.

{% hint style="warning" %}
**WebSocket-only topic.** Subscribe with [`getblock_subscribe`](getblock_subscribe-evm-data-stream.md) on any [network endpoint](./#endpoints).
{% endhint %}

## Parameters

<table data-search="false"><thead><tr><th>Parameter</th><th>Type</th><th>Required</th><th>Description</th></tr></thead><tbody>
<tr><td><code>filters.entryPoint</code></td><td>string</td><td>No</td><td>EntryPoint contract address. Omit to receive every canonical EntryPoint version.</td></tr>
<tr><td><code>filters.sender</code></td><td>string</td><td>No</td><td>Smart account (UserOperation <code>sender</code>).</td></tr>
<tr><td><code>filters.paymaster</code></td><td>string</td><td>No</td><td>Paymaster that sponsored the operation.</td></tr>
<tr><td><code>filters.userOpHash</code></td><td>string</td><td>No</td><td>One UserOperation hash.</td></tr>
<tr><td><code>filters.eventType</code></td><td>string</td><td>No</td><td>One of <code>userOperation</code>, <code>accountDeployed</code>, <code>revertReason</code>, or <code>beforeExecution</code>.</td></tr>
<tr><td><code>filters.success</code></td><td>boolean</td><td>No</td><td>Filter <code>userOperation</code> events by execution result.</td></tr>
</tbody></table>

`eventType` values map to the EntryPoint events:

| `eventType`        | EntryPoint event               | When it fires                                                       |
| ------------------ | ------------------------------ | ------------------------------------------------------------------- |
| `userOperation`    | `UserOperationEvent`           | Once per UserOperation, after execution, with its result and gas cost. |
| `accountDeployed`  | `AccountDeployed`              | When a UserOperation deploys its smart account.                     |
| `revertReason`     | `UserOperationRevertReason`    | When a UserOperation's call reverted, with the revert data.         |
| `beforeExecution`  | `BeforeExecution`              | Once per bundle, before the operations in it execute.               |

## Request Example

{% tabs %}
{% tab title="wscat" %}
{% code overflow="wrap" %}
```bash
# WebSocket-only. Connect first:
wscat -c 'wss://stream.eu-central-1.getblock.io/v1/eth-mainnet/stream' -H 'Authorization: Bearer <API-KEY>'

# Then send:
{"jsonrpc":"2.0","id":1,"method":"getblock_subscribe","params":[{"topic":"accountAbstractionOperations","filters":{"eventType":"userOperation","sender":"0xe21db522d0e27db4044486f656b6d26934c77645"}}]}
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
      topic: 'accountAbstractionOperations',
      filters: {
        eventType: 'userOperation',
        sender: '0xe21db522d0e27db4044486f656b6d26934c77645'
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
  if (data.eventType === 'userOperation') {
    console.log(removed ? 'REVERTED' : data.success ? 'userOp ok' : 'userOp FAILED', data.userOpHash, 'cost', BigInt(data.actualGasCost));
  }
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
                "topic": "accountAbstractionOperations",
                "filters": {
                    "eventType": "userOperation",
                    "sender": "0xe21db522d0e27db4044486f656b6d26934c77645"
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
            if data['eventType'] == 'userOperation':
                print('REVERTED' if removed else 'userOp ok' if data['success'] else 'userOp FAILED', data['userOpHash'], 'cost', int(data['actualGasCost'], 16))

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
  "result": "0xd83568be1b45519333164dbc949327bb"
}
```

Every later message is an event notification (a `userOperation` event):

```json
{
  "jsonrpc": "2.0",
  "method": "getblock_subscribe",
  "params": {
    "subscription": "0xd83568be1b45519333164dbc949327bb",
    "result": {
      "eventId": "eth:mainnet:receipts:0x830b6a483c183023519b834acfac62e7392ab7918c8…",
      "removed": false,
      "revision": 116877272,
      "data": {
        "eventType": "userOperation",
        "entryPoint": "0x4337084d9e255ff0702461cf8895ce9e3b5ff108",
        "userOpHash": "0x8657eaa065d117c15340470360d2b6e62160d1c5dca8f11ba978dc66f6f7a6ab",
        "sender": "0xe21db522d0e27db4044486f656b6d26934c77645",
        "paymaster": "0x888888888888ec68a58ab8094cc1ad20ba3d2402",
        "nonce": "0x1a0c3a931210000000000000000",
        "success": true,
        "actualGasCost": "0x1080d4b3292d0",
        "actualGasUsed": "0x5ebc6",
        "address": "0x4337084d9e255ff0702461cf8895ce9e3b5ff108",
        "blockNumber": "0x18d1e1f",
        "transactionHash": "0x54ebb6d71346068b99441bb55ddf2e8884175cf364eecbccc0a57ed297d6d770",
        "transactionIndex": "0x1ad",
        "blockHash": "0x830b6a483c183023519b834acfac62e7392ab7918c8f31b8d4188293e364d38f",
        "blockTimestamp": "0x6ab110cf",
        "logIndex": "0x252",
        "removed": false
      }
    }
  }
}
```

A `beforeExecution` event carries only the EntryPoint and log context:

```json
{
  "data": {
    "eventType": "beforeExecution",
    "entryPoint": "0x4337084d9e255ff0702461cf8895ce9e3b5ff108",
    "address": "0x4337084d9e255ff0702461cf8895ce9e3b5ff108",
    "blockNumber": "0x18d1e1f",
    "transactionHash": "0x54ebb6d71346068b99441bb55ddf2e8884175cf364eecbccc0a57ed297d6d770",
    "transactionIndex": "0x1ad",
    "blockHash": "0x830b6a483c183023519b834acfac62e7392ab7918c8f31b8d4188293e364d38f",
    "blockTimestamp": "0x6ab110cf",
    "logIndex": "0x248",
    "removed": false
  }
}
```

## Response Parameters

The `result` object contains the event envelope and the payload in `data`.

<table data-search="false"><thead><tr><th>Field</th><th>Type</th><th>Description</th></tr></thead><tbody>
<tr><td><code>eventId</code></td><td>string</td><td>Stable event identifier. A reorg correction reuses it.</td></tr>
<tr><td><code>removed</code></td><td>boolean</td><td><code>true</code> when a reorg removed this previously delivered event.</td></tr>
<tr><td><code>revision</code></td><td>number</td><td>Event version. For one <code>eventId</code>, a greater revision is newer.</td></tr>
<tr><td><code>data.eventType</code></td><td>string</td><td><code>userOperation</code>, <code>accountDeployed</code>, <code>revertReason</code>, or <code>beforeExecution</code>. The other fields present depend on it.</td></tr>
<tr><td><code>data.entryPoint</code></td><td>string</td><td>EntryPoint contract that emitted the event.</td></tr>
<tr><td><code>data.userOpHash</code></td><td>string</td><td>UserOperation hash. Not present on <code>beforeExecution</code>.</td></tr>
<tr><td><code>data.sender</code></td><td>string</td><td>Smart account address.</td></tr>
<tr><td><code>data.paymaster</code></td><td>string</td><td>Paymaster address, or the zero address when the account paid for itself.</td></tr>
<tr><td><code>data.nonce</code></td><td>string</td><td>UserOperation nonce, hex-encoded (<code>userOperation</code>).</td></tr>
<tr><td><code>data.success</code></td><td>boolean</td><td>Whether the UserOperation's call succeeded (<code>userOperation</code>).</td></tr>
<tr><td><code>data.actualGasCost</code></td><td>string</td><td>Total cost charged for the operation in wei, hex-encoded (<code>userOperation</code>).</td></tr>
<tr><td><code>data.actualGasUsed</code></td><td>string</td><td>Gas used by the operation, hex-encoded (<code>userOperation</code>).</td></tr>
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
`accountDeployed` and `revertReason` events carry the fields of their EntryPoint event (for example the account `factory`, or the `revertReason` bytes) alongside `userOpHash` and `sender`. Read fields by name and ignore any you don't use.
{% endhint %}

## Use Cases

* Confirming a UserOperation from your bundler or wallet executed, and whether it succeeded
* Sponsorship accounting for paymasters (filter by `paymaster`)
* Tracking smart-account onboarding (`accountDeployed`)
* Surfacing revert reasons to users of a smart wallet

## Error Handling

<table data-search="false"><thead><tr><th>Code</th><th>Message</th><th>Cause</th></tr></thead><tbody>
<tr><td><code>-32602</code></td><td><code>invalid eventType "…": must be userOperation, accountDeployed, revertReason, or beforeExecution</code></td><td><code>eventType</code> has an unsupported value.</td></tr>
<tr><td><code>-32602</code></td><td><code>invalid entryPoint: invalid EVM address "…"</code></td><td><code>entryPoint</code> is not a 20-byte hex address.</td></tr>
<tr><td><code>-32602</code></td><td><code>invalid sender: invalid EVM address "…"</code></td><td><code>sender</code> is not a 20-byte hex address.</td></tr>
<tr><td><code>-32602</code></td><td><code>invalid paymaster: invalid EVM address "…"</code></td><td><code>paymaster</code> is not a 20-byte hex address.</td></tr>
<tr><td><code>-32602</code></td><td><code>invalid accountAbstractionOperations request: json: unknown field "…"</code></td><td>The request has a field this topic does not accept.</td></tr>
<tr><td><code>-32001</code></td><td><code>subscription limit exceeded</code></td><td>You already have 15 active or pending subscriptions.</td></tr>
</tbody></table>
