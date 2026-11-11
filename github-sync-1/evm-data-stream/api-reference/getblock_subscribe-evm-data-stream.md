---
description: >-
  Open an EVM Data Stream subscription over WebSocket. Complete guide on how to
  use getblock_subscribe in GetBlock EVM Data Stream documentation.
---

# getblock\_subscribe - EVM Data Stream

`getblock_subscribe` opens a streaming subscription to one topic. The service replies with a subscription ID, then pushes a notification for every matching event until you unsubscribe or the socket closes.

{% hint style="warning" %}
**WebSocket-only method.** It does not work over HTTP POST.
{% endhint %}

## Parameters

`params` is an array containing exactly **one** request object. `topic` is always required; the other fields depend on the topic.

| Parameter    | Type    | Required        | Description                                                                                                   |
| ------------ | ------- | --------------- | ------------------------------------------------------------------------------------------------------------- |
| `topic`      | string  | Yes             | The topic to receive. One of the 15 [topics](./#topics).                                                      |
| `filters`    | object  | No              | Topic-specific filters. Used by the receipt and trace topics (`logs`, `erc20Transfers`, `newBlockTraces`, …). |
| `abi`        | array   | For `decodedLogs` | 1–16 event ABI definitions used to decode logs.                                                             |
| `hashesOnly` | boolean | No              | `newMinedTransactions` and `newPendingTransactions` only: return transaction hashes instead of full objects. |
| `addresses`  | array   | No              | `newMinedTransactions` only: up to 50 `{ from, to }` match rules.                                             |
| `fromAddress`, `toAddress` | string or array | No | `newPendingTransactions` only: sender and recipient addresses to match.                            |

Where each field goes:

<table data-search="false"><thead><tr><th>Topic</th><th>Accepted fields besides <code>topic</code></th></tr></thead><tbody>
<tr><td><code>newHeads</code>, <code>newBlocks</code></td><td>None. Any extra field is rejected.</td></tr>
<tr><td><code>logs</code></td><td><code>filters.address</code>, <code>filters.topics</code></td></tr>
<tr><td><code>decodedLogs</code></td><td><code>abi</code> (required), <code>filters.address</code></td></tr>
<tr><td><code>erc20Transfers</code>, <code>erc20Allowence</code></td><td><code>filters.tokenContract</code>, <code>filters.from</code>, <code>filters.to</code></td></tr>
<tr><td><code>nftTransfers</code></td><td><code>filters.tokenContract</code>, <code>filters.operator</code>, <code>filters.from</code>, <code>filters.to</code>, <code>filters.tokenId</code>, <code>filters.standard</code>, <code>filters.transferType</code></td></tr>
<tr><td><code>nftApprovals</code></td><td><code>filters.tokenContract</code>, <code>filters.owner</code>, <code>filters.operator</code>, <code>filters.tokenId</code>, <code>filters.approvalType</code></td></tr>
<tr><td><code>nftMetadataUpdates</code></td><td><code>filters.tokenContract</code>, <code>filters.tokenId</code></td></tr>
<tr><td><code>transactionReceipts</code></td><td><code>filters.status</code>, <code>filters.from</code>, <code>filters.to</code>, <code>filters.contractCreated</code>, <code>filters.transactionType</code></td></tr>
<tr><td><code>accountAbstractionOperations</code></td><td><code>filters.entryPoint</code>, <code>filters.sender</code>, <code>filters.paymaster</code>, <code>filters.userOpHash</code>, <code>filters.eventType</code>, <code>filters.success</code></td></tr>
<tr><td><code>newBlockTraces</code>, <code>newMinedTransactionsTraces</code></td><td><code>filters.transactionHash</code>, <code>filters.fromAddress</code>, <code>filters.toAddress</code>, <code>filters.callType</code>, <code>filters.status</code></td></tr>
<tr><td><code>newMinedTransactions</code></td><td><code>hashesOnly</code>, <code>addresses</code> (top level, not inside <code>filters</code>)</td></tr>
<tr><td><code>newPendingTransactions</code></td><td><code>hashesOnly</code>, <code>fromAddress</code>, <code>toAddress</code> (top level, not inside <code>filters</code>)</td></tr>
</tbody></table>

{% hint style="info" %}
Every filter is optional. Omitting all filters subscribes you to **every** matching event on the network, which on busy topics such as `erc20Transfers` or `logs` means many events per second, each billed at 10 CU. Filter as narrowly as your use case allows.
{% endhint %}

Requests are validated strictly:

* Unknown fields are rejected, at the top level and inside `filters`.
* `params` must be an array with exactly one object. Legacy positional params such as `["newHeads"]` are rejected.
* Historical fields (`fromBlock`, `toBlock`) and `includeRemoved` are not supported.

## Request Example

{% tabs %}
{% tab title="wscat" %}
{% code overflow="wrap" %}
```bash
# WebSocket-only. Connect first:
wscat -c 'wss://stream.eu-central-1.getblock.io/v1/eth-mainnet/stream' -H 'Authorization: Bearer <API-KEY>'

# Then send:
{"jsonrpc":"2.0","id":1,"method":"getblock_subscribe","params":[{"topic":"erc20Transfers","filters":{"tokenContract":"0xdAC17F958D2ee523a2206206994597C13D831ec7"}}]}
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
      topic: 'erc20Transfers',
      filters: { tokenContract: '0xdAC17F958D2ee523a2206206994597C13D831ec7' }
    }]
  }));
});

ws.on('message', (raw) => {
  const msg = JSON.parse(raw);

  if (msg.id !== undefined) {
    if (msg.error) console.error('Subscribe failed:', msg.error);
    else console.log('Subscribed:', msg.result);
    return;
  }

  const { eventId, removed, revision, data } = msg.params.result;
  console.log(removed ? 'REMOVED' : 'NEW', revision, eventId, data);
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
                "topic": "erc20Transfers",
                "filters": {"tokenContract": "0xdAC17F958D2ee523a2206206994597C13D831ec7"}
            }]
        }))

        async for message in ws:
            msg = json.loads(message)

            if "id" in msg:
                print("Subscribe failed:" if "error" in msg else "Subscribed:",
                      msg.get("error") or msg["result"])
                continue

            event = msg["params"]["result"]
            print("REMOVED" if event["removed"] else "NEW",
                  event["revision"], event["eventId"], event["data"])

asyncio.run(main())
```
{% endtab %}
{% endtabs %}

## Response Example

The first reply confirms the subscription and returns its ID:

```json
{
  "jsonrpc": "2.0",
  "id": 1,
  "result": "0x573e3382e8ea7d534f267b232aae22dd"
}
```

Every message after that is an event notification. It carries no `id`; the payload is under `params.result`:

```json
{
  "jsonrpc": "2.0",
  "method": "getblock_subscribe",
  "params": {
    "subscription": "0x573e3382e8ea7d534f267b232aae22dd",
    "result": {
      "eventId": "eth:mainnet:receipts:0x19ddc6864bd5ec1cdbd493e3d3653b529d79293a6773517018153cdff6ac0be2:erc20Transfers:0x19ddc6864bd5ec1cdbd493e3d3653b529d79293a6773517018153cdff6ac0be2:0x6b362543c83c1d18b27701235338c05a7e04c2e5098bed46d60afe9a0f17d2a1:0x3",
      "removed": false,
      "revision": 116876401,
      "data": {
        "tokenContract": "0xdac17f958d2ee523a2206206994597c13d831ec7",
        "from": "0x4b3dd3690ba06b2a73089e02381d0eba9a7dbca5",
        "to": "0xa2050435e59f41300b7787808e976255faa198e5",
        "value": "0x6f4b360",
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

| Field                    | Type             | Description                                                                                      |
| ------------------------ | ---------------- | ------------------------------------------------------------------------------------------------ |
| `result`                 | string           | Subscription ID, returned once in reply to the subscribe request. Treat it as opaque.            |
| `params.subscription`    | string           | The subscription a notification belongs to. Use it to demultiplex the socket.                     |
| `params.result.eventId`  | string           | Stable event identifier. Reorg corrections reuse it.                                             |
| `params.result.removed`  | boolean          | `true` when a reorg removed this previously delivered event.                                     |
| `params.result.revision` | number           | Event version. For one `eventId`, apply only a revision greater than the last one you applied.   |
| `params.result.data`     | object or string | Topic payload. See the topic's page.                                                             |

## Multiple subscriptions

One socket can carry several subscriptions. Send one `getblock_subscribe` per stream, map each returned ID to a handler, and route notifications on `params.subscription`:

```javascript
const pending = new Map();   // request id → handler
const handlers = new Map();  // subscription id → handler
let nextId = 1;

function subscribe(request, handler) {
  const id = nextId++;
  pending.set(id, handler);
  ws.send(JSON.stringify({ jsonrpc: '2.0', id, method: 'getblock_subscribe', params: [request] }));
}

ws.on('message', (raw) => {
  const msg = JSON.parse(raw);
  if (msg.id !== undefined) {
    if (typeof msg.result === 'string') handlers.set(msg.result, pending.get(msg.id));
    pending.delete(msg.id);
    return;
  }
  handlers.get(msg.params.subscription)?.(msg.params.result);
});

subscribe({ topic: 'newHeads' }, (event) => console.log('head', event.data.number));
subscribe({ topic: 'erc20Transfers', filters: { to: '0x…' } }, (event) => console.log('payment', event.data));
```

Every subscription counts toward the 15-subscription limit, whichever socket it is on.

## Error Handling

| Code     | Message                                                                                     | Cause                                                                                   |
| -------- | ------------------------------------------------------------------------------------------- | --------------------------------------------------------------------------------------- |
| `-32602` | `getblock_subscribe requires exactly one request object`                                    | `params` is empty or holds more than one object.                                        |
| `-32602` | `invalid subscription request: json: cannot unmarshal string into Go value of type …`       | `params` holds a string, as in the legacy `["newHeads"]` form.                          |
| `-32602` | `unsupported subscription topic "…"`                                                        | `topic` is not one of the 15 topics.                                                    |
| `-32602` | `invalid <topic> request: json: unknown field "…"`                                          | The request has a field the topic does not accept, such as `filters` on `newHeads`, or `fromBlock`. |
| `-32602` | `invalid <field>: invalid EVM address "…"`                                                  | An address filter is not a 20-byte hex address.                                         |
| `-32001` | `subscription limit exceeded`                                                               | You already have 15 active or pending subscriptions. Other subscriptions are unaffected. |
| `-32000` | —                                                                                           | The backing live source could not create the subscription. Retry with backoff.          |
| `-32601` | `method not found`                                                                          | The method name is wrong, for example `eth_subscribe`.                                  |

{% hint style="danger" %}
`params` must be an **array**. If you send the request object directly (`"params": {"topic": "newHeads"}`), the server does not reply at all. Your client waits forever for a subscription ID.
{% endhint %}
