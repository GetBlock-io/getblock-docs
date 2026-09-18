---
description: >-
  Example code for the subscribeNewBlock WebSocket method. Complete guide on how to use
  the subscribeNewBlock WebSocket method in the GetBlock Web3 documentation.
---

# subscribeNewBlock - Dogecoin

Subscribes to new blocks. The server pushes the height and hash of each block as it is connected to the chain. Combined with a transaction's own block height, this is how confirmation depth is tracked without polling.

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
        "height": 6378961,
        "hash": "7603e59cec9c7a411b052acc187b27def866ef5d4653711a619c57801a65dfbf"
    }
}
```

## Response Fields

| Field | Type | Description |
| --- | --- | --- |
| subscribed | boolean | Confirms the subscription is active |
| height | integer | Height of the newly connected block (in notifications) |
| hash | string | Hash of the newly connected block (in notifications) |

{% hint style="warning" %}
During a chain reorganization a notification can carry a height equal to or lower than one already delivered. Treat the height as the current tip rather than a counter that only increases, and re-check any transaction that has not reached final settlement depth.

Dogecoin targets a **one-minute** block interval, about ten times Bitcoin's rate, so these notifications arrive far more often and a depth copied from a Bitcoin integration settles in roughly a tenth of the wall-clock time. Choose depth by the time you need, not by copying a Bitcoin figure.
{% endhint %}

## Use Cases

* **Confirmation Counting**: Compute depth as the new height minus a transaction's block height, plus one
* **Settlement Triggers**: Release an order once a deposit reaches the required depth
* **Chain Tip Tracking**: Keep a local view of the best height current
* **Cache Invalidation**: Refresh indexed data when a block arrives

## Error Handling

| Error | Message | Description |
| --- | --- | --- |
| 403 / RBAC: access denied | Access denied | The GetBlock access token is missing or incorrect |
