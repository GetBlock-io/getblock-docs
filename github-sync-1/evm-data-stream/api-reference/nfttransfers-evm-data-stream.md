---
description: >-
  Stream normalized ERC-721 and ERC-1155 transfers, mints, and burns. Complete guide on how to use the nftTransfers topic in GetBlock EVM Data Stream documentation.
---

# nftTransfers - EVM Data Stream

The `nftTransfers` topic normalizes ERC-721 `Transfer` and ERC-1155 `TransferSingle` and `TransferBatch` events into **one item per token moved**, labelled as a transfer, mint, or burn. You get the same shape whichever standard the collection uses.

{% hint style="warning" %}
**WebSocket-only topic.** Subscribe with [`getblock_subscribe`](getblock_subscribe-evm-data-stream.md) on any [network endpoint](./#endpoints).
{% endhint %}

## Parameters

<table data-search="false"><thead><tr><th>Parameter</th><th>Type</th><th>Required</th><th>Description</th></tr></thead><tbody>
<tr><td><code>filters.tokenContract</code></td><td>string</td><td>No</td><td>NFT contract address.</td></tr>
<tr><td><code>filters.from</code></td><td>string</td><td>No</td><td>Sender address.</td></tr>
<tr><td><code>filters.to</code></td><td>string</td><td>No</td><td>Recipient address.</td></tr>
<tr><td><code>filters.operator</code></td><td>string</td><td>No</td><td>ERC-1155 operator: the address that executed the transfer. Only ERC-1155 events carry an operator.</td></tr>
<tr><td><code>filters.tokenId</code></td><td>string</td><td>No</td><td>One token ID as a hex quantity, for example <code>"0x14e2e4"</code>. Decimal strings are rejected.</td></tr>
<tr><td><code>filters.standard</code></td><td>string</td><td>No</td><td><code>erc721</code> or <code>erc1155</code>.</td></tr>
<tr><td><code>filters.transferType</code></td><td>string</td><td>No</td><td><code>transfer</code>, <code>mint</code> (from the zero address), or <code>burn</code> (to the zero address).</td></tr>
</tbody></table>

## Request Example

{% tabs %}
{% tab title="wscat" %}
{% code overflow="wrap" %}
```bash
# WebSocket-only. Connect first:
wscat -c 'wss://stream.eu-central-1.getblock.io/v1/eth-mainnet/stream' -H 'Authorization: Bearer <API-KEY>'

# Then send:
{"jsonrpc":"2.0","id":1,"method":"getblock_subscribe","params":[{"topic":"nftTransfers","filters":{"tokenContract":"0xc36442b4a4522e871399cd717abdd847ab11fe88","transferType":"mint"}}]}
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
      topic: 'nftTransfers',
      filters: {
        tokenContract: '0xc36442b4a4522e871399cd717abdd847ab11fe88',
        transferType: 'mint'
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
  console.log(removed ? 'REVERTED' : data.transferType, data.standard, data.tokenContract, 'id', BigInt(data.tokenId).toString(), '→', data.to);
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
                "topic": "nftTransfers",
                "filters": {
                    "tokenContract": "0xc36442b4a4522e871399cd717abdd847ab11fe88",
                    "transferType": "mint"
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
            print('REVERTED' if removed else data['transferType'], data['standard'], data['tokenContract'], 'id', int(data['tokenId'], 16), '→', data['to'])

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
  "result": "0x90665748bf0dc9af925b430bbf29ae3d"
}
```

Every later message is an event notification (an ERC-721 mint):

```json
{
  "jsonrpc": "2.0",
  "method": "getblock_subscribe",
  "params": {
    "subscription": "0x90665748bf0dc9af925b430bbf29ae3d",
    "result": {
      "eventId": "eth:mainnet:receipts:0x830b6a483c183023519b834acfac62e7392ab7918c8…",
      "removed": false,
      "revision": 116877272,
      "data": {
        "standard": "erc721",
        "tokenContract": "0xc36442b4a4522e871399cd717abdd847ab11fe88",
        "from": "0x0000000000000000000000000000000000000000",
        "to": "0x32ce7c9bc35243aba1d6a06933ec4081b2117881",
        "tokenId": "0x14e2e4",
        "amount": "0x1",
        "transferType": "mint",
        "address": "0xc36442b4a4522e871399cd717abdd847ab11fe88",
        "blockNumber": "0x18d1e1f",
        "transactionHash": "0xe0623924811bb927416695a43800b6647a53984bf85effbe094e26f04bdc1f2d",
        "transactionIndex": "0xb3",
        "blockHash": "0x830b6a483c183023519b834acfac62e7392ab7918c8f31b8d4188293e364d38f",
        "blockTimestamp": "0x6ab110cf",
        "logIndex": "0x135",
        "removed": false
      }
    }
  }
}
```

An ERC-1155 transfer carries an `operator` and an `amount` greater than one:

```json
{
  "data": {
    "standard": "erc1155",
    "tokenContract": "0xe8bd225aab19cd3cc0e98bd510e4b2fab91247a4",
    "operator": "0xf4002e2535785ae17f28d2ed894fecb69b0d8f75",
    "from": "0xf4002e2535785ae17f28d2ed894fecb69b0d8f75",
    "to": "0x7f6f77c8ecc3828e02cd0bc0d40b24e61291cfbd",
    "tokenId": "0xb4ae194a0dcf1b4080b164c1d775ee06e0817305",
    "amount": "0x64",
    "transferType": "transfer",
    "address": "0xe8bd225aab19cd3cc0e98bd510e4b2fab91247a4",
    "blockNumber": "0x18d1ece",
    "transactionHash": "0x29f2dedcb2b82553ab82356ca9aa07d4c67e77505d03f390b5af935dac489245",
    "transactionIndex": "0x74",
    "blockHash": "0x3fee5dee0da3eecd8fc862fe5ced2f950207a4fbe16f466c370d4785c4dbc0cd",
    "blockTimestamp": "0x6ab1191b",
    "logIndex": "0xca",
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
<tr><td><code>data.standard</code></td><td>string</td><td><code>erc721</code> or <code>erc1155</code>.</td></tr>
<tr><td><code>data.transferType</code></td><td>string</td><td><code>transfer</code>, <code>mint</code>, or <code>burn</code>.</td></tr>
<tr><td><code>data.tokenContract</code></td><td>string</td><td>NFT contract address.</td></tr>
<tr><td><code>data.from</code></td><td>string</td><td>Sender. The zero address for a mint.</td></tr>
<tr><td><code>data.to</code></td><td>string</td><td>Recipient. The zero address for a burn.</td></tr>
<tr><td><code>data.operator</code></td><td>string</td><td>ERC-1155 only: the address that executed the transfer.</td></tr>
<tr><td><code>data.tokenId</code></td><td>string</td><td>Token ID, hex-encoded. Convert with <code>BigInt(tokenId)</code>; IDs can exceed 2^53.</td></tr>
<tr><td><code>data.amount</code></td><td>string</td><td>Number of tokens moved, hex-encoded. Always <code>0x1</code> for ERC-721.</td></tr>
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
An ERC-1155 `TransferBatch` becomes one notification per token ID in the batch. They share `transactionHash` and `logIndex` but have different `eventId` values.
{% endhint %}

## Use Cases

* NFT portfolio and wallet views that update in real time
* Mint trackers and drop monitors (`transferType: "mint"`)
* Marketplace and collection activity feeds
* Game-item inventories on ERC-1155 contracts

## Error Handling

<table data-search="false"><thead><tr><th>Code</th><th>Message</th><th>Cause</th></tr></thead><tbody>
<tr><td><code>-32602</code></td><td><code>invalid standard "…": must be erc721 or erc1155</code></td><td><code>standard</code> has an unsupported value.</td></tr>
<tr><td><code>-32602</code></td><td><code>invalid transferType "…": must be transfer, mint, or burn</code></td><td><code>transferType</code> has an unsupported value.</td></tr>
<tr><td><code>-32602</code></td><td><code>… cannot unmarshal hex string without 0x prefix …</code></td><td>A hex-quantity filter was sent as a decimal string. Encode it as <code>0x…</code>.</td></tr>
<tr><td><code>-32602</code></td><td><code>invalid tokenContract: invalid EVM address "…"</code></td><td><code>tokenContract</code> is not a 20-byte hex address.</td></tr>
<tr><td><code>-32602</code></td><td><code>invalid nftTransfers request: json: unknown field "…"</code></td><td>The request has a field this topic does not accept.</td></tr>
<tr><td><code>-32001</code></td><td><code>subscription limit exceeded</code></td><td>You already have 15 active or pending subscriptions.</td></tr>
</tbody></table>
