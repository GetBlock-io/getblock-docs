---
description: >-
  Example code for the subscribeNewBlock WebSocket method. Complete guide on how to use
  the subscribeNewBlock WebSocket method in the GetBlock Web3 documentation.
---

# subscribeNewBlock - Zcash

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

While subscribed, the server pushes one message per connected block. The message below was captured live:

```json
{
    "id": "newblock",
    "data": {
        "height": 3489838,
        "hash": "00000000008bcf8e9d872a7dfec09b908a61bd1edc8c783dfb0959118ab920c5"
    }
}
```

The push carries only the height and hash. Fetch the block itself with REST [api/v2/block](../zcash-blockbook-rest-api/api-v2-block-zcash.md) if its transactions are needed.

## Response Fields

| Field | Type | Description |
| --- | --- | --- |
| subscribed | boolean | Confirms the subscription is active |
| height | integer | Height of the newly connected block (in notifications) |
| hash | string | Hash of the newly connected block (in notifications) |

{% hint style="warning" %}
During a chain reorganization a notification can carry a height equal to or lower than one already delivered. Treat the height as the current tip rather than a counter that only increases, and re-check any transaction that has not reached final settlement depth.

Zcash targets a **75-second** block interval, so these arrive about eight times as often as on Bitcoin and a confirmation depth copied from a Bitcoin integration settles in roughly an eighth of the wall-clock time.
{% endhint %}

{% hint style="info" %}
Pair this with [subscribeAddresses](subscribeaddresses-zcash.md) for payment flows. An address push tells you a deposit arrived; a block push tells you when to re-read its depth. Zcash transactions also expire: compare the tip height against a pending transaction's `expiryheight`, after which it can no longer be mined.
{% endhint %}

## Use Cases

* **Confirmation Counting**: Compute depth as the new height minus a transaction's block height, plus one
* **Settlement Triggers**: Release an order once a deposit reaches the required depth
* **Chain Tip Tracking**: Keep a local view of the best height current
* **Expiry Watching**: Compare the tip against a pending transaction's `expiryheight`

## Error Handling

| Error | Message | Description |
| --- | --- | --- |
| 403 / RBAC: access denied | Access denied | The GetBlock access token is missing or incorrect |
