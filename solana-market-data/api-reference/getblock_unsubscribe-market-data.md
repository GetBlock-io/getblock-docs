---
description: >-
  Cancel an active Solana Market Data subscription. Complete guide on how to use
  getblock_unsubscribe in GetBlock Solana Market Data documentation.
---

# getblock\_unsubscribe - Solana Market Data

`getblock_unsubscribe` cancels a subscription created with [`getblock_subscribe`](getblock_subscribe-market-data.md). Delivery for that subscription stops immediately and the subscription ID is no longer valid. Other subscriptions on the same socket are unaffected.

{% hint style="warning" %}
**WebSocket-only method.** Send it on the same connection that owns the subscription.
{% endhint %}

## Parameters

`params` is an array containing exactly **one object**:

| Parameter      | Type   | Required | Description                                                        |
| -------------- | ------ | -------- | -------------------------------------------------------------------- |
| `subscription` | string | Yes      | The subscription ID returned by `getblock_subscribe`.              |

{% hint style="danger" %}
The subscription ID must be wrapped in an object — `[{"subscription": "0x…"}]`. Passing the ID as a bare string, as Solana's own `*Unsubscribe` methods accept, fails with `getblock_unsubscribe params must contain one object`.
{% endhint %}

## Request Example

{% tabs %}
{% tab title="wscat" %}
{% code overflow="wrap" %}
```bash
# WebSocket-only. On the connection that owns the subscription, send:
{"jsonrpc":"2.0","id":"getblock.io","method":"getblock_unsubscribe","params":[{"subscription":"0x4503d281ca474f3322ed277fde75db15"}]}
```
{% endcode %}
{% endtab %}

{% tab title="JavaScript" %}
```javascript
ws.send(JSON.stringify({
  jsonrpc: '2.0',
  id: 'getblock.io',
  method: 'getblock_unsubscribe',
  params: [{ subscription: subscriptionId }]
}));
```
{% endtab %}

{% tab title="Python" %}
```python
await ws.send(json.dumps({
    "jsonrpc": "2.0",
    "id": "getblock.io",
    "method": "getblock_unsubscribe",
    "params": [{"subscription": subscription_id}]
}))
```
{% endtab %}
{% endtabs %}

## Response Example

```json
{
  "jsonrpc": "2.0",
  "id": "getblock.io",
  "result": true
}
```

## Response Parameters

| Field    | Type    | Description                                                                          |
| -------- | ------- | -------------------------------------------------------------------------------------- |
| `result` | boolean | `true` when the subscription was cancelled. No further notifications are delivered for it. |

## Use Cases

* Switching a chart or dashboard from one market pair or window to another
* Releasing streams a user has navigated away from without dropping the socket
* Managing several subscriptions on one connection independently
* Tearing down cleanly on component unmount

## Error Handling

| Code     | Message                                                | Cause                                                                  |
| -------- | ------------------------------------------------------ | ------------------------------------------------------------------------ |
| `-32602` | `getblock_unsubscribe params must contain one object`  | `params` was empty, held more than one entry, or the entry was not an object — for example a bare ID string. |
| `-32602` | `subscription is required`                             | The object was passed without a `subscription` field. Note that `id` is not an accepted alias. |
