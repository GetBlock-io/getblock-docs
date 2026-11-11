---
description: >-
  GetBlock webhooks deliver at least once and send reorg corrections. How to
  de-duplicate deliveries and undo events whose block left the chain.
---

# Handling Chain Reorganizations

A chain reorganization (reorg) replaces recent blocks with another branch. An event you received in the unconfirmed phase can therefore disappear from the chain. GetBlock tells you when that happens, and your handler has to undo what it did for that event.

### At-least-once delivery

Delivery is **at least once**: the same event can arrive more than once, for example when your answer was lost and GetBlock retried.

**De-duplicate on `eventId` + phase.** Each `eventId` arrives at most once per phase:

| Phase | `confirmed` | `removed` |
| --- | --- | --- |
| Unconfirmed | `false` | `false` |
| Confirmed | `true` | `false` |
| Removed (reorg correction) | `false` | `true` |

A webhook that takes both phases receives two deliveries per event, so de-duplicating on `eventId` alone would drop the confirmed one.

`eventId` is opaque: store and compare it as a string, do not parse it.

### Reorg corrections

* **`"removed": true`** means the block holding the event left the canonical chain. Undo whatever you did for that `eventId`. The correction carries the same `eventId` and the same payload as the original delivery.
* A correction can arrive for an `eventId` you never stored — treat it as a no-op. Corrections are matched against the webhook's current settings, so a webhook created, resumed or re-filtered between an event and its reorg can receive the removal of an event it never got. Such a delivery is billed like any other attempt.
* The same transfer re-mined in another block arrives as a **new** event with a **new** `eventId`, possibly before the removal of the old one.
* CU spent on deliveries are not returned when a reorg removes the event.

### Which phase to subscribe to

Corrections are delivered with the **unconfirmed** phase only.

* A webhook that takes `confirmed` only is **not** told about a reorg that happens after its confirmed delivery. Include `unconfirmed` if you need to hear about late reorgs.
* A reorg that happens before the confirmation depth is reached simply cancels the confirmed delivery: the event never arrives as confirmed.

A common pattern for payments: show a payment as pending when the unconfirmed delivery arrives, credit it when the confirmed delivery arrives, and roll it back on a correction.

### Handling example

{% code overflow="wrap" %}
```javascript
// event: the parsed body of a delivery whose signature you have already verified
async function handleEvent(event, db) {
  const phase = event.removed ? "removed" : event.confirmed ? "confirmed" : "unconfirmed";

  // At-least-once delivery: skip a phase of an event you have already handled
  if (await db.seen(event.eventId, phase)) return;
  await db.markSeen(event.eventId, phase);

  if (event.removed) {
    // The block left the chain: undo the effects of this eventId.
    // A removal for an eventId you never stored is a no-op.
    await db.revert(event.eventId);
  } else if (event.confirmed) {
    // Final enough to act on: credit the payment
    await db.confirm(event.eventId, event);
  } else {
    // First inclusion: show it as pending
    await db.recordPending(event.eventId, event);
  }
}
```
{% endcode %}

For the full payload of each delivery see [Delivery Format](delivery-format.md#example).
