---
description: >-
  Example code for the subscribeNewBlock WebSocket method. Complete guide on how
  to use the subscribeNewBlock WebSocket method in the GetBlock Web3
  documentation.
---

# subscribeNewBlock - Litecoin

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
        "height": 3179816,
        "hash": "3e469b185937924138e5ab9b4fda4901dc2ad5507c7190fb56968c0654a0a1ac"
    }
}
```

## Response Fields

| Field      | Type    | Description                                            |
| ---------- | ------- | ------------------------------------------------------ |
| subscribed | boolean | Confirms the subscription is active                    |
| height     | integer | Height of the newly connected block (in notifications) |
| hash       | string  | Hash of the newly connected block (in notifications)   |

{% hint style="warning" %}
During a chain reorganization a notification can carry a height equal to or lower than one already delivered. Treat the height as the current tip rather than a counter that only increases, and re-check any transaction that has not reached final settlement depth.

Litecoin targets a 2.5-minute block interval, roughly four times Bitcoin's rate, so a confirmation policy ported from Bitcoin by block count settles in about a quarter of the wall-clock time. Choose depth by the time you need, not by copying a Bitcoin figure.
{% endhint %}

## Use Cases

* **Confirmation Counting**: Compute depth as the new height minus a transaction's block height, plus one
* **Settlement Triggers**: Release an order once a deposit reaches the required depth
* **Chain Tip Tracking**: Keep a local view of the best height current
* **Cache Invalidation**: Refresh indexed data when a block arrives

## Error Handling

| Error                     | Message       | Description                                       |
| ------------------------- | ------------- | ------------------------------------------------- |
| 403 / RBAC: access denied | Access denied | The GetBlock access token is missing or incorrect |
