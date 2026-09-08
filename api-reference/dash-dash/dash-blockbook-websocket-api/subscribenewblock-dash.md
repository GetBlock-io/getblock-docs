---
description: >-
  Example code for the subscribeNewBlock WebSocket method. Complete guide on how
  to use the subscribeNewBlock WebSocket method in the GetBlock Web3
  documentation.
---

# subscribeNewBlock - Dash

Subscribes to new block notifications over WebSocket. After subscribing, the server pushes a message with the height and hash of each new block as it is indexed.

{% hint style="warning" %}
This is a WebSocket subscription. After the initial acknowledgement, the server pushes notifications until you unsubscribe or disconnect.
{% endhint %}

## Parameters

{% hint style="info" %}
This method takes no parameters.
{% endhint %}

## Message

{% code overflow="wrap" %}
```bash
wscat -c wss://shared.eu-central-1.getblock.io/<ACCESS-TOKEN>/websocket

# then send:
{
    "id": "getblock.io",
    "method": "subscribeNewBlock",
    "params": {}
}
```
{% endcode %}

## Response

```json
{
    "id": "getblock.io",
    "data": {
        "subscribed": true
    }
}
```

## Notifications

While subscribed, the server pushes messages of the form:

```json
{
    "id": "getblock.io",
    "data": {
        "height": 2100001,
        "hash": "000000000000000abc12def34567890fedcba9876543210abcdef1234567890ff"
    }
}
```

## Response Fields

| Field      | Type    | Description                                |
| ---------- | ------- | ------------------------------------------ |
| subscribed | boolean | Confirms the subscription is active        |
| height     | number  | Height of the new block (in notifications) |
| hash       | string  | Hash of the new block (in notifications)   |

## Use Cases

* **Chain Following**: React to each new block live
* **Indexers**: Trigger ingestion on new blocks
* **Dashboards**: Live height display

## Error Handling

| Error                     | Message            | Description                                       |
| ------------------------- | ------------------ | ------------------------------------------------- |
| error                     | Subscription error | The subscription could not be established         |
| 403 / RBAC: access denied | Access denied      | The GetBlock access token is missing or incorrect |
