---
description: >-
  Example code for the subscribeNewBlock WebSocket method. Complete guide on how
  to use the subscribeNewBlock WebSocket method in the GetBlock Web3
  documentation.
---

# subscribeNewBlock - Bitcoin

Subscribes to new blocks. The server pushes the height and hash of each block as it is connected to the chain. Combined with a transaction's own block height, this is how confirmation depth is tracked without polling.

{% hint style="warning" %}
This is a WebSocket subscription. After the initial acknowledgement, the server pushes notifications until the connection unsubscribes or disconnects.
{% endhint %}

## Parameters

This method takes no parameters. Send an empty `params` object.

## Message

{% code overflow="wrap" %}
```bash
wscat -c wss://shared.eu-central-1.getblock.io/<ACCESS-TOKEN>

# then send:
{
    "id": "newblock",
    "method": "subscribeNewBlock",
    "params": {}
}
```
{% endcode %}

## Response

```json
{
    "id": "newblock",
    "data": {
        "subscribed": true
    }
}
```

## Notifications

While subscribed, the server pushes one message per connected block:

```json
{
    "id": "newblock",
    "data": {
        "height": 967488,
        "hash": "000000000000000000004f71d28efeab2a13c7e8ba2f2946c20506d93ecd6f53"
    }
}
```

## Response Fields

| Field      | Type    | Description                                            |
| ---------- | ------- | ------------------------------------------------------ |
| subscribed | boolean | Confirms the subscription is active                    |
| height     | integer | Height of the newly connected block (in notifications) |
| hash       | string  | Hash of the newly connected block (in notifications)   |

{% hint style="info" %}
Notifications reuse the `id` sent with the subscription request, not a fixed value. Give each subscription on a connection its own `id` — `newblock` above — so pushes can be routed by `id` without inspecting the payload.
{% endhint %}

## Use Cases

* **Confirmation Counting**: Compute depth as the new height minus a transaction's block height, plus one
* **Settlement Triggers**: Release an order once a deposit reaches the required depth
* **Chain Tip Tracking**: Keep a local view of the best height current
* **Cache Invalidation**: Refresh indexed data when a block arrives

{% hint style="warning" %}
During a chain reorganization, a notification can carry a height equal to or lower than one already delivered. Treat the height as the current tip rather than a counter that only increases, and re-check the confirmation depth of any transaction that has not reached final settlement depth.
{% endhint %}

## Error Handling

| Error                     | Message       | Description                                       |
| ------------------------- | ------------- | ------------------------------------------------- |
| 403 / RBAC: access denied | Access denied | The GetBlock access token is missing or incorrect |
