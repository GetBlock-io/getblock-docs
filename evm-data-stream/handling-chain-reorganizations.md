---
description: >-
  How EVM Data Stream reports chain reorganizations on the original
  subscription, what delivery guarantees apply, and how to reconcile events on
  the client.
---

# Handling Chain Reorganizations

A block that looked canonical can be replaced by a competing block a few seconds later. Every event the stream delivered from the replaced block (headers, transactions, receipts, logs, transfers, and traces) is then no longer true.

EVM Data Stream reports this on the **same subscription** that delivered the original event. There is no separate reorg topic and no `includeRemoved` option: corrections are always on.

{% hint style="warning" %}
Reorg corrections are currently enabled on **Ethereum Mainnet** and **Polygon Mainnet**. Support on BNB Smart Chain and Robinhood Mainnet will be announced separately. Until then, apply your own confirmation depth on those networks if you need reorg safety.
{% endhint %}

## The event envelope

Every event, including a correction, carries the same four fields:

| Field      | Type    | Description                                                                                         |
| ---------- | ------- | --------------------------------------------------------------------------------------------------- |
| `eventId`  | string  | Stable identifier for the event. A correction reuses the `eventId` of the event it removes.         |
| `removed`  | boolean | `false` for a canonical event, `true` for a correction that removes a previously delivered event.   |
| `revision` | number  | Version number. For one `eventId`, a greater revision is always the newer state.                    |
| `data`     | any     | The topic payload. A correction repeats the payload of the event it removes.                        |

A delivered event and its later correction:

{% tabs %}
{% tab title="Original event" %}
```json
{
  "eventId": "eth:mainnet:block:0x19ddc6864bd5ec1cdbd493e3d3653b529d79293a6773517018153cdff6ac0be2:newHeads:0x19ddc6864bd5ec1cdbd493e3d3653b529d79293a6773517018153cdff6ac0be2",
  "removed": false,
  "revision": 116876399,
  "data": { "number": "0x18d1e1e", "hash": "0x19ddc6864bd5ec1cdbd493e3d3653b529d79293a6773517018153cdff6ac0be2", "…": "…" }
}
```
{% endtab %}

{% tab title="Reorg correction" %}
```json
{
  "eventId": "eth:mainnet:block:0x19ddc6864bd5ec1cdbd493e3d3653b529d79293a6773517018153cdff6ac0be2:newHeads:0x19ddc6864bd5ec1cdbd493e3d3653b529d79293a6773517018153cdff6ac0be2",
  "removed": true,
  "revision": 116876512,
  "data": { "number": "0x18d1e1e", "hash": "0x19ddc6864bd5ec1cdbd493e3d3653b529d79293a6773517018153cdff6ac0be2", "…": "…" }
}
```
{% endtab %}
{% endtabs %}

## How to apply events

1. Store every `removed: false` event by `eventId`.
2. When the same `eventId` arrives with `removed: true`, undo or delete the stored value.
3. Events from the new canonical branch may arrive **before or after** the removals. Apply every event by `eventId` and keep the one with the greater `revision`.
4. Ignore any event whose `revision` is not greater than the one you already applied for that `eventId`. This also makes duplicate deliveries harmless.

A minimal client reducer:

```javascript
const revisions = new Map();
const state = new Map();

function applyEvent(event) {
  const previous = revisions.get(event.eventId);
  if (previous !== undefined && event.revision <= previous) return; // already applied

  revisions.set(event.eventId, event.revision);

  if (event.removed) {
    state.delete(event.eventId);
  } else {
    state.set(event.eventId, event.data);
  }
}

ws.on('message', (raw) => {
  const msg = JSON.parse(raw);
  if (msg.method === 'getblock_subscribe') applyEvent(msg.params.result);
});
```

{% hint style="info" %}
Don't want corrections? Ignore events with `removed: true`. The stream sends them regardless.
{% endhint %}

## What is guaranteed

* **Corrections are always on.** A client that does not want them ignores `removed: true`.
* **Corrections pass the same filters** as the original event. A filtered subscription never receives a correction for an event it could not have received.
* **A correction is sent only for an event this subscription received.**
* **Corrections are ordered.** For one reorg, corrections go from the orphaned tip downward. Within a block, traces come before receipts, and receipts come before the block.
* **Delivery is at-least-once.** A repeated event is safe to apply because `eventId` is stable and `revision` identifies the newer state.

## What is not guaranteed

* Events of the new canonical branch may arrive **before** the old branch is removed.
* Exactly-once delivery.
* A common order across different subscriptions on one socket, or across API replicas.
* Replay of events published while you were disconnected, **corrections included**. After a reconnect, rebuild state from your own source (for example, an RPC node) before trusting the stream again.

## Finality

Inclusion in a block is not finality. The stream delivers events as soon as blocks are observed, and a correction may follow. If your application must act only on final data, such as crediting a deposit, wait for your own confirmation depth or for the network's finalized block before acting, and use corrections to reverse anything you showed provisionally.
