---
description: >-
  The HTTPS request a GetBlock webhook sends to your endpoint: headers,
  response rules, payload fields, example events and test deliveries.
---

# Delivery Format

Every delivery is one HTTPS `POST` to your webhook's `target_url` with one event as its JSON body.

### The request

{% code overflow="wrap" %}
```http
POST <target_url>
Content-Type: application/json
User-Agent: GetBlock-Webhooks/1
X-GetBlock-Signature: t=1789455600,v1=663d17a7e5aa07ca7df21050f7688cc54a78e4fa9cfbb5d1774e4df5e31822d4
```
{% endcode %}

* These three headers are the ones GetBlock sets; the usual transport headers (`Host`, `Content-Length`, `Accept-Encoding`) come with them. How to check `X-GetBlock-Signature` is on [Verifying Signatures](verifying-signatures.md).
* The body is **one event** as uncompressed JSON. There is no batching and no `Content-Encoding`.
* Your endpoint must present a valid TLS certificate for its host name.
* Answer with any `2xx` within **15 seconds** (5 seconds to connect and finish the TLS handshake). The response body is not used; at most 4 KiB of it is read.
* **Redirects are never followed:** a `3xx` answer is a failed attempt.

Anything other than a `2xx` in time is retried. See [Retries and Endpoint Protection](retries-and-endpoint-protection.md).

### Payload fields

| Field | Type | Meaning |
| --- | --- | --- |
| `webhookId` | string | The webhook (`wh_…`) |
| `eventId` | string | The event's identity, the same in an event and its reorg correction. Opaque: compare it as a string, do not parse it |
| `type` | string | `address_activity`, `log_event` or `tx_confirmation` |
| `network` | string | The network, for example `eth-mainnet` |
| `confirmed` | boolean | `false` — first inclusion in a block; `true` — the block reached the confirmation depth |
| `removed` | boolean | `true` — reorg correction: the block holding this event left the canonical chain |
| `block` | object | `number` (hex), `hash` and an optional `timestamp` (hex) |
| `raw` | object | The log or transaction exactly as the node returned it |
| `decoded` | object, optional | `event`, `signature` and `params` (name → value; `{}` when the event has no parameters). Present for ERC-20 `Transfer` and `Approval` |

* Optional keys are left out, never sent as `null`.
* `confirmed` and `removed` are never both `true`.
* Numbers are hex strings, as the node returns them.

### Example

An ERC-20 `Transfer` delivered by a `log_event` webhook that takes both phases. The three deliveries of one event share the same `eventId`:

{% tabs %}
{% tab title="Unconfirmed" %}
{% code overflow="wrap" %}
```json
{
  "webhookId": "wh_9f0c2d7e",
  "eventId": "eth:mainnet:receipts:0x3805a4c919e7023ab2dc9be9b9bdf2bc44e0c157028948967c5f5f6f227baa64:0x1b6e7b7715daac0582a3039eed9b453d6bf520765b9dc4da77a4a83d457c4083:0",
  "type": "log_event",
  "network": "eth-mainnet",
  "confirmed": false,
  "removed": false,
  "block": {
    "number": "0xac9f38",
    "hash": "0x3805a4c919e7023ab2dc9be9b9bdf2bc44e0c157028948967c5f5f6f227baa64",
    "timestamp": "0x6a5e1ef4"
  },
  "raw": {
    "address": "0x5b6fade3b2d59d01d4f996aed530332165bb391e",
    "topics": [
      "0xddf252ad1be2c89b69c2b068fc378daa952ba7f163c4a11628f55a4df523b3ef",
      "0x000000000000000000000000691f834aeeb8167551a94d933fc6a66011f38ad3",
      "0x0000000000000000000000002f1528f344f6410d361654d22a47ef121a60c938"
    ],
    "data": "0x00000000000000000000000000000000000000000000000135dd58a691664000",
    "blockNumber": "0xac9f38",
    "transactionHash": "0x1b6e7b7715daac0582a3039eed9b453d6bf520765b9dc4da77a4a83d457c4083",
    "transactionIndex": "0x0",
    "blockHash": "0x3805a4c919e7023ab2dc9be9b9bdf2bc44e0c157028948967c5f5f6f227baa64",
    "blockTimestamp": "0x6a5e1ef4",
    "logIndex": "0x0",
    "removed": false
  },
  "decoded": {
    "event": "Transfer",
    "signature": "Transfer(address,address,uint256)",
    "params": {
      "from": "0x691f834aeeb8167551a94d933fc6a66011f38ad3",
      "to": "0x2f1528f344f6410d361654d22a47ef121a60c938",
      "value": "0x135dd58a691664000"
    }
  }
}
```
{% endcode %}
{% endtab %}

{% tab title="Confirmed" %}
{% code overflow="wrap" %}
```json
{
  "webhookId": "wh_9f0c2d7e",
  "eventId": "eth:mainnet:receipts:0x3805a4c919e7023ab2dc9be9b9bdf2bc44e0c157028948967c5f5f6f227baa64:0x1b6e7b7715daac0582a3039eed9b453d6bf520765b9dc4da77a4a83d457c4083:0",
  "type": "log_event",
  "network": "eth-mainnet",
  "confirmed": true,
  "removed": false,
  "block": {
    "number": "0xac9f38",
    "hash": "0x3805a4c919e7023ab2dc9be9b9bdf2bc44e0c157028948967c5f5f6f227baa64",
    "timestamp": "0x6a5e1ef4"
  },
  "raw": {
    "address": "0x5b6fade3b2d59d01d4f996aed530332165bb391e",
    "topics": [
      "0xddf252ad1be2c89b69c2b068fc378daa952ba7f163c4a11628f55a4df523b3ef",
      "0x000000000000000000000000691f834aeeb8167551a94d933fc6a66011f38ad3",
      "0x0000000000000000000000002f1528f344f6410d361654d22a47ef121a60c938"
    ],
    "data": "0x00000000000000000000000000000000000000000000000135dd58a691664000",
    "blockNumber": "0xac9f38",
    "transactionHash": "0x1b6e7b7715daac0582a3039eed9b453d6bf520765b9dc4da77a4a83d457c4083",
    "transactionIndex": "0x0",
    "blockHash": "0x3805a4c919e7023ab2dc9be9b9bdf2bc44e0c157028948967c5f5f6f227baa64",
    "blockTimestamp": "0x6a5e1ef4",
    "logIndex": "0x0",
    "removed": false
  },
  "decoded": {
    "event": "Transfer",
    "signature": "Transfer(address,address,uint256)",
    "params": {
      "from": "0x691f834aeeb8167551a94d933fc6a66011f38ad3",
      "to": "0x2f1528f344f6410d361654d22a47ef121a60c938",
      "value": "0x135dd58a691664000"
    }
  }
}
```
{% endcode %}
{% endtab %}

{% tab title="Reorg correction" %}
{% code overflow="wrap" %}
```json
{
  "webhookId": "wh_9f0c2d7e",
  "eventId": "eth:mainnet:receipts:0x3805a4c919e7023ab2dc9be9b9bdf2bc44e0c157028948967c5f5f6f227baa64:0x1b6e7b7715daac0582a3039eed9b453d6bf520765b9dc4da77a4a83d457c4083:0",
  "type": "log_event",
  "network": "eth-mainnet",
  "confirmed": false,
  "removed": true,
  "block": {
    "number": "0xac9f38",
    "hash": "0x3805a4c919e7023ab2dc9be9b9bdf2bc44e0c157028948967c5f5f6f227baa64",
    "timestamp": "0x6a5e1ef4"
  },
  "raw": {
    "address": "0x5b6fade3b2d59d01d4f996aed530332165bb391e",
    "topics": [
      "0xddf252ad1be2c89b69c2b068fc378daa952ba7f163c4a11628f55a4df523b3ef",
      "0x000000000000000000000000691f834aeeb8167551a94d933fc6a66011f38ad3",
      "0x0000000000000000000000002f1528f344f6410d361654d22a47ef121a60c938"
    ],
    "data": "0x00000000000000000000000000000000000000000000000135dd58a691664000",
    "blockNumber": "0xac9f38",
    "transactionHash": "0x1b6e7b7715daac0582a3039eed9b453d6bf520765b9dc4da77a4a83d457c4083",
    "transactionIndex": "0x0",
    "blockHash": "0x3805a4c919e7023ab2dc9be9b9bdf2bc44e0c157028948967c5f5f6f227baa64",
    "blockTimestamp": "0x6a5e1ef4",
    "logIndex": "0x0",
    "removed": true
  },
  "decoded": {
    "event": "Transfer",
    "signature": "Transfer(address,address,uint256)",
    "params": {
      "from": "0x691f834aeeb8167551a94d933fc6a66011f38ad3",
      "to": "0x2f1528f344f6410d361654d22a47ef121a60c938",
      "value": "0x135dd58a691664000"
    }
  }
}
```
{% endcode %}
{% endtab %}
{% endtabs %}

The confirmed delivery is identical to the unconfirmed one except `"confirmed": true`. The correction carries the same `eventId` and payload with `"removed": true`. What to do with each is on [Handling Chain Reorganizations](handling-chain-reorganizations.md).

### Other triggers

{% tabs %}
{% tab title="address_activity" %}
A native ETH transfer to or from a watched address. A token transfer of a watched address arrives as the token's log, with `decoded` for ERC-20:

{% code overflow="wrap" %}
```json
{
  "webhookId": "wh_9f0c2d7e",
  "eventId": "eth:mainnet:block:0x8cb2f70c45c47f4a2ec42b725377f1275c814335167226717a71367e87fda143:0x4117679e70b37fb2eb5efbbbf27fbc46f898dd87971390b468ea37a3efb83c74:native:0",
  "type": "address_activity",
  "network": "eth-mainnet",
  "confirmed": false,
  "removed": false,
  "block": {
    "number": "0xac9f5b",
    "hash": "0x8cb2f70c45c47f4a2ec42b725377f1275c814335167226717a71367e87fda143",
    "timestamp": "0x6a5e20b0"
  },
  "raw": {
    "hash": "0x4117679e70b37fb2eb5efbbbf27fbc46f898dd87971390b468ea37a3efb83c74",
    "blockNumber": "0xac9f5b",
    "blockHash": "0x8cb2f70c45c47f4a2ec42b725377f1275c814335167226717a71367e87fda143",
    "transactionIndex": "0x1",
    "from": "0x9948d016d36a6d9972da41c30697ade901b24e18",
    "to": "0xfa6419a3d3503a016df3a59f690734862ca2a78d",
    "value": "0xde0b6b3a7640000",
    "nonce": "0x2b"
  }
}
```
{% endcode %}
{% endtab %}

{% tab title="tx_confirmation" %}
`raw` is the transaction and `eventId` refers to the block it was mined in; `block` is the newer block at which the transaction reached `confirm_depth`:

{% code overflow="wrap" %}
```json
{
  "webhookId": "wh_9f0c2d7e",
  "eventId": "eth:mainnet:receipts:0x3805a4c919e7023ab2dc9be9b9bdf2bc44e0c157028948967c5f5f6f227baa64:0x1b6e7b7715daac0582a3039eed9b453d6bf520765b9dc4da77a4a83d457c4083:conf:12",
  "type": "tx_confirmation",
  "network": "eth-mainnet",
  "confirmed": true,
  "removed": false,
  "block": {
    "number": "0xac9f44",
    "hash": "0x93f28f7b1e6a4d5c2b0a9e8d7c6b5a4f3e2d1c0b9a8f7e6d5c4b3a2f1e0d9c8b"
  },
  "raw": {
    "hash": "0x1b6e7b7715daac0582a3039eed9b453d6bf520765b9dc4da77a4a83d457c4083",
    "blockNumber": "0xac9f38",
    "blockHash": "0x3805a4c919e7023ab2dc9be9b9bdf2bc44e0c157028948967c5f5f6f227baa64",
    "transactionIndex": "0x0",
    "from": "0x691f834aeeb8167551a94d933fc6a66011f38ad3",
    "to": "0x5b6fade3b2d59d01d4f996aed530332165bb391e",
    "value": "0x0",
    "nonce": "0x1a"
  }
}
```
{% endcode %}
{% endtab %}
{% endtabs %}

### Test deliveries

A test event (**Send test event** in the dashboard, or `POST /api/v1/webhooks/{id}/test`) is a delivery of the webhook's own event type, signed with the webhook's secrets and sent to its URL with the same retries and protection as a live event.

* The data is sample mainnet data. Its `eventId` contains `:test:`, and every test is a new `eventId`. There is no `block.timestamp`, and `confirmed` is `true`.
* Test deliveries are **free**, and are sent even when your CU balance is zero.
* Only an `active` webhook can be tested: otherwise the answer is `409 webhook_not_active`.
* `503 test_event_unavailable` means the test could not be confirmed as queued in time; it may still arrive. Another request sends a second, separate test event.
* A successful test delivery shows in the webhook's stats (`delivered_total`), not in the delivery history, which lists failed attempts only.
* Test deliveries count like any other delivery for the circuit breaker and auto-pause: repeated tests against an endpoint that fails bring an automatic pause closer.
