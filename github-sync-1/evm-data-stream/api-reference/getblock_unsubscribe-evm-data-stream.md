---
description: >-
  Cancel an active EVM Data Stream subscription. Complete guide on how to use
  getblock_unsubscribe in GetBlock EVM Data Stream documentation.
---

# getblock\_unsubscribe - EVM Data Stream

`getblock_unsubscribe` cancels a subscription created with [`getblock_subscribe`](getblock_subscribe-evm-data-stream.md). Delivery for that subscription stops and it no longer counts toward your 15-subscription limit. Other subscriptions on the same socket are unaffected.

{% hint style="warning" %}
**WebSocket-only method.** Send it on the same connection that owns the subscription.
{% endhint %}

## Parameters

`params` is an array containing exactly **one object**:

| Parameter      | Type   | Required | Description                                           |
| -------------- | ------ | -------- | ----------------------------------------------------- |
| `subscription` | string | Yes      | The subscription ID returned by `getblock_subscribe`. |

{% hint style="danger" %}
Wrap the subscription ID in an object: `[{"subscription": "0x…"}]`. A bare string, as `eth_unsubscribe` accepts, fails with `getblock_unsubscribe params must contain one object`.
{% endhint %}

## Request Example

{% tabs %}
{% tab title="wscat" %}
{% code overflow="wrap" %}
```bash
wscat -c 'wss://stream.eu-central-1.getblock.io/v1/eth-mainnet/stream' -H 'Authorization: Bearer <API-KEY>'
# WebSocket-only. On the connection that owns the subscription, send:
{"jsonrpc":"2.0","id":2,"method":"getblock_unsubscribe","params":[{"subscription":"0x573e3382e8ea7d534f267b232aae22dd"}]}
```
{% endcode %}
{% endtab %}

{% tab title="JavaScript" %}
```javascript
ws.send(JSON.stringify({
  jsonrpc: '2.0',
  id: 2,
  method: 'getblock_unsubscribe',
  params: [{ subscription: subscriptionId }]
}));
```
{% endtab %}

{% tab title="Python" %}
```python
await ws.send(json.dumps({
    "jsonrpc": "2.0",
    "id": 2,
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
  "id": 2,
  "result": true
}
```

## Response Parameters

| Field    | Type    | Description                                                                                                                                   |
| -------- | ------- | --------------------------------------------------------------------------------------------------------------------------------------------- |
| `result` | boolean | `true` when the subscription was cancelled. `false` when no active subscription with that ID exists on this connection, for example because it was already cancelled. |

{% hint style="info" %}
An event that was already in flight can still arrive shortly after you unsubscribe. Drop notifications whose `params.subscription` you no longer track.
{% endhint %}

## Use Cases

* Switching a wallet view from one address to another without dropping the socket
* Freeing a slot when you approach the 15-subscription limit
* Replacing a subscription with one that has different filters (subscriptions cannot be modified in place)
* Tearing down cleanly when a component unmounts

## Error Handling

| Code     | Message                                               | Cause                                                                                                  |
| -------- | ----------------------------------------------------- | ------------------------------------------------------------------------------------------------------ |
| `-32602` | `getblock_unsubscribe params must contain one object` | `params` was empty, held more than one entry, or the entry was not an object, such as a bare ID string. |
| `-32602` | `subscription is required`                            | The object has no `subscription` field. `id` is not an accepted alias.                                  |
