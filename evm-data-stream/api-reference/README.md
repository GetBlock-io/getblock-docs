---
description: >-
  Full reference for the GetBlock EVM Data Stream WebSocket API: endpoints,
  authentication, methods, topics, the event envelope, limits, and errors.
---

# API Reference

This section documents every method and topic exposed by the EVM Data Stream WebSocket API.

The API has two methods: [`getblock_subscribe`](getblock_subscribe-evm-data-stream.md) and [`getblock_unsubscribe`](getblock_unsubscribe-evm-data-stream.md). The [**topic**](./#topics) you subscribe to decides what you receive. Each topic has its own page covering its parameters, a live sample response, and every field it returns.

### Quickstart

Subscribe to new block headers on Ethereum Mainnet:

{% tabs %}
{% tab title="Request" %}
```json
{
  "jsonrpc": "2.0",
  "id": 1,
  "method": "getblock_subscribe",
  "params": [{ "topic": "newHeads" }]
}
```
{% endtab %}

{% tab title="Response" %}
The service replies with a subscription ID, then pushes one notification per event:

```json
{
  "jsonrpc": "2.0",
  "method": "getblock_subscribe",
  "params": {
    "subscription": "0xfd5126364b7ac0fbe8ad9099ce29571c",
    "result": {
      "eventId": "eth:mainnet:block:0x19dd…0be2:newHeads:0x19dd…0be2",
      "removed": false,
      "revision": 116876399,
      "data": { "number": "0x18d1e1e", "hash": "0x19ddc6864bd5ec1cdbd493e3d3653b529d79293a6773517018153cdff6ac0be2", "…": "…" }
    }
  }
}
```
{% endtab %}
{% endtabs %}

### Endpoints

Each network has its own WebSocket endpoint. Every topic is available on every network.

| Network             | WebSocket endpoint                                                      |
| ------------------- | ----------------------------------------------------------------------- |
| Ethereum Mainnet    | `wss://stream.eu-central-1.getblock.io/v1/eth-mainnet/stream`           |
| BNB Smart Chain     | `wss://stream.eu-central-1.getblock.io/v1/bsc-mainnet/stream`           |
| Polygon Mainnet     | `wss://stream.eu-central-1.getblock.io/v1/polygon-mainnet/stream`       |
| Robinhood Mainnet   | `wss://stream.eu-central-1.getblock.io/v1/robinhood-mainnet/stream`     |

### Authentication

Every connection needs a GetBlock API key (`gb_…`). Create one in [Settings → API Keys](https://account.getblock.io/). Pass it in one of two ways:

{% tabs %}
{% tab title="Authorization header (server-side)" %}
Use this whenever your WebSocket client can set custom headers: Node.js, Python, Go, `wscat`, and so on.

{% code overflow="wrap" %}
```bash
wscat -c 'wss://stream.eu-central-1.getblock.io/v1/eth-mainnet/stream' \
  -H 'Authorization: Bearer <API-KEY>'
```
{% endcode %}
{% endtab %}

{% tab title="apiKey query parameter (browser)" %}
Browser `WebSocket` APIs cannot set an `Authorization` header, so add the key to the URL instead:

{% code overflow="wrap" %}
```
wss://stream.eu-central-1.getblock.io/v1/eth-mainnet/stream?apiKey=<API-KEY>
```
{% endcode %}
{% endtab %}
{% endtabs %}

{% hint style="warning" %}
Credentials in a URL can end up in browser history, proxy and gateway logs, or telemetry. Prefer the `Authorization` header outside the browser, and never log the full query URL.
{% endhint %}

The server rejects authentication failures during the WebSocket handshake:

| HTTP status | Body                   | Cause                                          |
| ----------- | ---------------------- | ---------------------------------------------- |
| `401`       | `authorization failed` | No API key was sent.                           |
| `401`       | `invalid api key`      | The API key is not recognized.                 |
| `404`       | —                      | The network in the URL path is not supported.  |
| `429`       | —                      | You already have 50 open connections.          |

{% hint style="info" %}
**The socket closes immediately with code `1006` and no reason.** Browser and Node.js `WebSocket` clients cannot read the handshake response, so every rejection above surfaces as close code `1006`. Check the API key, the network in the path, and your open connection count.
{% endhint %}

### Methods

| Method                                                              | Description                                                                 |
| ------------------------------------------------------------------- | --------------------------------------------------------------------------- |
| [`getblock_subscribe`](getblock_subscribe-evm-data-stream.md)       | Open a subscription to one topic. Returns a subscription ID, then streams events. |
| [`getblock_unsubscribe`](getblock_unsubscribe-evm-data-stream.md)   | Cancel one subscription without closing the socket.                         |

### Topics

<table data-search="false"><thead><tr><th>Topic</th><th>Group</th><th>What each notification carries</th></tr></thead><tbody>
<tr><td><a href="newheads-evm-data-stream.md"><code>newHeads</code></a></td><td>Blocks</td><td>A block header</td></tr>
<tr><td><a href="newblocks-evm-data-stream.md"><code>newBlocks</code></a></td><td>Blocks</td><td>A full block with transaction objects</td></tr>
<tr><td><a href="logs-evm-data-stream.md"><code>logs</code></a></td><td>Logs</td><td>One raw receipt log</td></tr>
<tr><td><a href="decodedlogs-evm-data-stream.md"><code>decodedLogs</code></a></td><td>Logs</td><td>One log decoded with the ABI you supply</td></tr>
<tr><td><a href="erc20transfers-evm-data-stream.md"><code>erc20Transfers</code></a></td><td>Tokens</td><td>A decoded ERC-20 <code>Transfer</code> event</td></tr>
<tr><td><a href="erc20allowence-evm-data-stream.md"><code>erc20Allowence</code></a></td><td>Tokens</td><td>A decoded ERC-20 <code>Approval</code> event</td></tr>
<tr><td><a href="nfttransfers-evm-data-stream.md"><code>nftTransfers</code></a></td><td>NFTs</td><td>A normalized ERC-721 or ERC-1155 transfer, mint, or burn</td></tr>
<tr><td><a href="nftapprovals-evm-data-stream.md"><code>nftApprovals</code></a></td><td>NFTs</td><td>A normalized NFT token or operator approval</td></tr>
<tr><td><a href="nftmetadataupdates-evm-data-stream.md"><code>nftMetadataUpdates</code></a></td><td>NFTs</td><td>A decoded ERC-4906 metadata update</td></tr>
<tr><td><a href="transactionreceipts-evm-data-stream.md"><code>transactionReceipts</code></a></td><td>Transactions</td><td>One raw transaction receipt</td></tr>
<tr><td><a href="accountabstractionoperations-evm-data-stream.md"><code>accountAbstractionOperations</code></a></td><td>Transactions</td><td>A decoded ERC-4337 EntryPoint event</td></tr>
<tr><td><a href="newminedtransactions-evm-data-stream.md"><code>newMinedTransactions</code></a></td><td>Transactions</td><td>A mined transaction, or only its hash</td></tr>
<tr><td><a href="newpendingtransactions-evm-data-stream.md"><code>newPendingTransactions</code></a></td><td>Transactions</td><td>A mempool transaction, or only its hash</td></tr>
<tr><td><a href="newblocktraces-evm-data-stream.md"><code>newBlockTraces</code></a></td><td>Traces</td><td>A block's matching call traces</td></tr>
<tr><td><a href="newminedtransactionstraces-evm-data-stream.md"><code>newMinedTransactionsTraces</code></a></td><td>Traces</td><td>One mined transaction's call trace</td></tr>
</tbody></table>

{% hint style="warning" %}
`erc20Allowence` is the current public topic name, including the misspelling. Send it exactly as shown; `erc20Allowance` is rejected.
{% endhint %}

### Choosing a topic

<table data-search="false"><thead><tr><th>If you need</th><th>Use</th></tr></thead><tbody>
<tr><td>A signal every time a block is produced</td><td><code>newHeads</code></td></tr>
<tr><td>Every transaction in each block, in one message</td><td><code>newBlocks</code></td></tr>
<tr><td>Incoming token payments to a wallet</td><td><code>erc20Transfers</code> with <code>filters.to</code></td></tr>
<tr><td>Alerts when someone grants a token allowance</td><td><code>erc20Allowence</code></td></tr>
<tr><td>A contract's events as raw topics and data</td><td><code>logs</code></td></tr>
<tr><td>A contract's events with named, typed arguments</td><td><code>decodedLogs</code></td></tr>
<tr><td>NFT mints, burns, and transfers</td><td><code>nftTransfers</code></td></tr>
<tr><td>Marketplace listing approvals, or <code>setApprovalForAll</code> alerts</td><td><code>nftApprovals</code></td></tr>
<tr><td>A signal to refresh cached NFT metadata</td><td><code>nftMetadataUpdates</code></td></tr>
<tr><td>Success or failure of transactions and their gas cost</td><td><code>transactionReceipts</code></td></tr>
<tr><td>Smart-account (ERC-4337) activity</td><td><code>accountAbstractionOperations</code></td></tr>
<tr><td>Confirmation that a transaction was included in a block</td><td><code>newMinedTransactions</code></td></tr>
<tr><td>Transactions before they are mined</td><td><code>newPendingTransactions</code></td></tr>
<tr><td>Internal calls and contract-to-contract value flow</td><td><code>newMinedTransactionsTraces</code> or <code>newBlockTraces</code></td></tr>
</tbody></table>

### Concepts that apply to every topic

#### 1. The event envelope

Every notification uses the same envelope. The topic-specific payload sits in `params.result.data`:

```json
{
  "jsonrpc": "2.0",
  "method": "getblock_subscribe",
  "params": {
    "subscription": "0x573e3382e8ea7d534f267b232aae22dd",
    "result": {
      "eventId": "eth:mainnet:receipts:0x19dd…0be2:erc20Transfers:0x19dd…0be2:0x6b36…d2a1:0x3",
      "removed": false,
      "revision": 116876401,
      "data": { "…": "topic-specific payload" }
    }
  }
}
```

| Field                     | Type    | Description                                                                                                                  |
| ------------------------- | ------- | ---------------------------------------------------------------------------------------------------------------------------- |
| `params.subscription`     | string  | The subscription this event belongs to. Use it to route events when one socket carries several subscriptions.               |
| `params.result.eventId`   | string  | Stable identifier for this event. A reorg correction for the event carries the same `eventId`. Treat it as opaque.           |
| `params.result.removed`   | boolean | `false` for a new event. `true` when a chain reorganization removed an event you already received.                           |
| `params.result.revision`  | number  | Monotonic version number. For the same `eventId`, a higher revision is newer.                                                |
| `params.result.data`      | object or string | The topic payload. See each topic's page.                                                                           |

{% hint style="info" %}
Several log-derived payloads also contain their own `data.removed` field, mirroring the `removed` field of an Ethereum log. Base your reorg handling on the envelope's `result.removed`.
{% endhint %}

#### 2. Reorg corrections

When a block leaves the canonical chain, the stream sends every affected event again on the **same subscription** with `removed: true` and a higher `revision`. There is no separate reorg topic and no option to switch corrections off. Store events by `eventId` and keep the one with the greatest `revision`. See [Handling chain reorganizations](../handling-chain-reorganizations.md) for the full contract and a client reducer.

{% hint style="warning" %}
Reorg corrections are currently enabled on **Ethereum** and **Polygon**. BNB Smart Chain and Robinhood will be announced separately.
{% endhint %}

#### 3. Value encoding

Numeric values such as block numbers, amounts, gas, token IDs, and indexes are returned as EVM hex quantities (`"0x18d1e1e"`), the same as the Ethereum JSON-RPC API. Addresses in normalized payloads are lowercase; argument values in `decodedLogs` keep their EIP-55 checksum casing. Ignore unknown fields so that new ones don't break your client.

#### 4. Live-only delivery

The stream carries live events only. Historical ranges (`fromBlock`, `toBlock`) and `includeRemoved` are rejected, and events published while you were disconnected are not replayed. After a reconnect, rebuild state from your own source before relying on the stream again.

#### 5. One socket, many subscriptions

Send several `getblock_subscribe` requests on one connection and route notifications on `params.subscription`. Event order across different subscriptions is not guaranteed.

### Connection keepalive

The server sends a WebSocket ping about every 30 seconds, and standard clients answer it automatically, so an idle connection with no events stays open.

{% hint style="warning" %}
**Python `websockets`: set `ping_interval=None`.** The server's pong replies do not echo the ping payload. Python `websockets` checks the payload, treats the pong as missing, and closes the connection with `1011 keepalive ping timeout` after about 40 seconds without events. Disabling client pings avoids this; the server's own pings keep the connection alive. Node.js `ws` and browser clients are not affected.
{% endhint %}

### Limits

These per-user limits apply across all networks and topics:

| Limit                                 | Default | When exceeded                                                                                                  |
| ------------------------------------- | ------- | -------------------------------------------------------------------------------------------------------------- |
| Open WebSocket connections            | 50      | The WebSocket upgrade is rejected with HTTP `429`. No socket is opened.                                         |
| Active or pending subscriptions       | 15      | Only the new subscribe request fails, with `-32001 subscription limit exceeded`. The socket and its other subscriptions stay open. |
| Addresses per transaction filter      | 50      | The subscribe request fails with `-32602`.                                                                     |

### Pricing

Every delivered event costs **10 CU**, on every network and topic. You are billed for events after filters are applied, so precise filters directly reduce cost. Each subscription is billed separately: the same event delivered to two subscriptions counts twice.

### Errors

Handshake failures use an HTTP status (see [Authentication](./#authentication)). Requests on an open socket return standard JSON-RPC error objects:

| Code     | Meaning                                                  | Example message                                                    |
| -------- | -------------------------------------------------------- | ------------------------------------------------------------------ |
| `-32700` | The message is not valid JSON                            | `parse error`                                                      |
| `-32600` | Invalid JSON-RPC request                                 | —                                                                  |
| `-32601` | Method is not supported                                  | `method not found`                                                 |
| `-32602` | Subscription params failed validation                    | `unsupported subscription topic "nope"`                            |
| `-32000` | The backing live source could not create the subscription | —                                                                 |
| `-32001` | Per-user active or pending subscription limit reached    | `subscription limit exceeded`                                      |

Each topic page lists the `-32602` messages specific to its parameters.
