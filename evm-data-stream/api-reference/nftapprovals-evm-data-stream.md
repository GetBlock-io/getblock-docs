---
description: >-
  Stream normalized NFT token approvals and operator (setApprovalForAll) approvals. Complete guide on how to use the nftApprovals topic in GetBlock EVM Data Stream documentation.
---

# nftApprovals - EVM Data Stream

The `nftApprovals` topic reports two kinds of NFT permission changes. A **token** approval is ERC-721 `Approval`: one address may move one specific token. An **operator** approval is `ApprovalForAll`: an address may move every token the owner holds in a collection.

{% hint style="warning" %}
**WebSocket-only topic.** Subscribe with [`getblock_subscribe`](getblock_subscribe-evm-data-stream.md) on any [network endpoint](./#endpoints).
{% endhint %}

## Parameters

<table data-search="false"><thead><tr><th>Parameter</th><th>Type</th><th>Required</th><th>Description</th></tr></thead><tbody>
<tr><td><code>filters.tokenContract</code></td><td>string</td><td>No</td><td>NFT contract address.</td></tr>
<tr><td><code>filters.owner</code></td><td>string</td><td>No</td><td>Token owner granting the approval.</td></tr>
<tr><td><code>filters.operator</code></td><td>string</td><td>No</td><td>Operator being approved. Applies to operator approvals.</td></tr>
<tr><td><code>filters.tokenId</code></td><td>string</td><td>No</td><td>One token ID as a hex quantity. Applies to token approvals.</td></tr>
<tr><td><code>filters.approvalType</code></td><td>string</td><td>No</td><td><code>token</code> or <code>operator</code>.</td></tr>
</tbody></table>

## Request Example

{% tabs %}
{% tab title="wscat" %}
{% code overflow="wrap" %}
```bash
# WebSocket-only. Connect first:
wscat -c 'wss://stream.eu-central-1.getblock.io/v1/eth-mainnet/stream' -H 'Authorization: Bearer <API-KEY>'

# Then send:
{"jsonrpc":"2.0","id":1,"method":"getblock_subscribe","params":[{"topic":"nftApprovals","filters":{"approvalType":"operator","owner":"0x71b187ecfa0a952bfbafe2393d60898ecf9d2150"}}]}
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
      topic: 'nftApprovals',
      filters: {
        approvalType: 'operator',
        owner: '0x71b187ecfa0a952bfbafe2393d60898ecf9d2150'
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
  if (data.approvalType === 'operator') {
    console.log(removed ? 'REVERTED' : data.approved ? 'operator approved' : 'operator revoked', data.operator, 'on', data.tokenContract);
  } else {
    console.log(removed ? 'REVERTED' : 'token approval', data.tokenContract, data.tokenId, '→', data.approvedAddress);
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
                "topic": "nftApprovals",
                "filters": {
                    "approvalType": "operator",
                    "owner": "0x71b187ecfa0a952bfbafe2393d60898ecf9d2150"
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
            if data['approvalType'] == 'operator':
                print('REVERTED' if removed else 'operator approved' if data['approved'] else 'operator revoked', data['operator'], 'on', data['tokenContract'])
            else:
                print('REVERTED' if removed else 'token approval', data['tokenContract'], data['tokenId'], '→', data['approvedAddress'])

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
  "result": "0x1ce16f57d8a6cd94787f746bf3df6a97"
}
```

Every later message is an event notification (an operator approval):

```json
{
  "jsonrpc": "2.0",
  "method": "getblock_subscribe",
  "params": {
    "subscription": "0x1ce16f57d8a6cd94787f746bf3df6a97",
    "result": {
      "eventId": "eth:mainnet:receipts:0x9c0b5d6fd93faa9293c4512f34fe470149dbb78481d…",
      "removed": false,
      "revision": 117088395,
      "data": {
        "standard": "unknown",
        "tokenContract": "0xd539a3a5edb713e6587e559a9d007ffff92bd9ab",
        "approvalType": "operator",
        "owner": "0x71b187ecfa0a952bfbafe2393d60898ecf9d2150",
        "operator": "0x1e0049783f008a0085193e00003d00cd54003c71",
        "approved": true,
        "address": "0xd539a3a5edb713e6587e559a9d007ffff92bd9ab",
        "blockNumber": "0x18d1ec6",
        "transactionHash": "0x45bee04d65959c74e6a54a4c887cc18bd78b9e01c8bdc7afe070a32c9db41387",
        "transactionIndex": "0x57",
        "blockHash": "0x9c0b5d6fd93faa9293c4512f34fe470149dbb78481dc20fa5b1bb444401ff029",
        "blockTimestamp": "0x6ab118af",
        "logIndex": "0xca",
        "removed": false
      }
    }
  }
}
```

A token approval carries `tokenId` and `approvedAddress` instead of `operator` and `approved`:

```json
{
  "data": {
    "standard": "erc721",
    "tokenContract": "0x524cab2ec69124574082676e6f654a18df49a048",
    "approvalType": "token",
    "owner": "0xb5479d539096b67ad412b3421043d320da4fe245",
    "tokenId": "0x3b9a",
    "approvedAddress": "0x0000000000000000000000000000000000000000",
    "address": "0x524cab2ec69124574082676e6f654a18df49a048",
    "blockNumber": "0x18d1e1f",
    "transactionHash": "0xa7268a62b15120df3c4c4dee5aa7b9d260550e9060e38e0da5f262bf07e63d1f",
    "transactionIndex": "0x1f4",
    "blockHash": "0x830b6a483c183023519b834acfac62e7392ab7918c8f31b8d4188293e364d38f",
    "blockTimestamp": "0x6ab110cf",
    "logIndex": "0x2ee",
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
<tr><td><code>data.approvalType</code></td><td>string</td><td><code>token</code> or <code>operator</code>.</td></tr>
<tr><td><code>data.standard</code></td><td>string</td><td><code>erc721</code> for token approvals. <code>unknown</code> for operator approvals, because ERC-721 and ERC-1155 share the same <code>ApprovalForAll</code> event.</td></tr>
<tr><td><code>data.tokenContract</code></td><td>string</td><td>NFT contract address.</td></tr>
<tr><td><code>data.owner</code></td><td>string</td><td>Owner who granted or changed the approval.</td></tr>
<tr><td><code>data.tokenId</code></td><td>string</td><td>Token approvals only: the token ID, hex-encoded.</td></tr>
<tr><td><code>data.approvedAddress</code></td><td>string</td><td>Token approvals only: the address approved for the token. The zero address clears the approval.</td></tr>
<tr><td><code>data.operator</code></td><td>string</td><td>Operator approvals only: the operator address.</td></tr>
<tr><td><code>data.approved</code></td><td>boolean</td><td>Operator approvals only: <code>true</code> when granted, <code>false</code> when revoked.</td></tr>
<tr><td><code>data.address</code></td><td>string</td><td>Address of the contract that emitted the log.</td></tr>
<tr><td><code>data.blockNumber</code></td><td>string</td><td>Block number, hex-encoded.</td></tr>
<tr><td><code>data.blockHash</code></td><td>string</td><td>Hash of the block that contains the log.</td></tr>
<tr><td><code>data.blockTimestamp</code></td><td>string</td><td>Block timestamp in Unix seconds, hex-encoded.</td></tr>
<tr><td><code>data.transactionHash</code></td><td>string</td><td>Hash of the transaction that emitted the log.</td></tr>
<tr><td><code>data.transactionIndex</code></td><td>string</td><td>Transaction's position in the block, hex-encoded.</td></tr>
<tr><td><code>data.logIndex</code></td><td>string</td><td>Log's position in the block, hex-encoded.</td></tr>
<tr><td><code>data.removed</code></td><td>boolean</td><td>The underlying log's <code>removed</code> flag. Use the envelope's <code>removed</code> for reorg handling.</td></tr>
</tbody></table>

Many ERC-721 contracts emit a token `Approval` to the zero address on every transfer to clear the previous approval, so expect a large share of token approvals with `approvedAddress` set to zero.

## Use Cases

* Security alerts when a wallet grants `setApprovalForAll` to an unknown operator
* Marketplace listing detection: owners approve the marketplace as operator before listing
* Revoke tools and approval dashboards
* Compliance monitoring of custody wallets

## Error Handling

<table data-search="false"><thead><tr><th>Code</th><th>Message</th><th>Cause</th></tr></thead><tbody>
<tr><td><code>-32602</code></td><td><code>invalid approvalType "…": must be token or operator</code></td><td><code>approvalType</code> has an unsupported value.</td></tr>
<tr><td><code>-32602</code></td><td><code>… cannot unmarshal hex string without 0x prefix …</code></td><td>A hex-quantity filter was sent as a decimal string. Encode it as <code>0x…</code>.</td></tr>
<tr><td><code>-32602</code></td><td><code>invalid owner: invalid EVM address "…"</code></td><td><code>owner</code> is not a 20-byte hex address.</td></tr>
<tr><td><code>-32602</code></td><td><code>invalid nftApprovals request: json: unknown field "…"</code></td><td>The request has a field this topic does not accept.</td></tr>
<tr><td><code>-32001</code></td><td><code>subscription limit exceeded</code></td><td>You already have 15 active or pending subscriptions.</td></tr>
</tbody></table>
