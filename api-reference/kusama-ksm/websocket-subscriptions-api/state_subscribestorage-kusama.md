---
description: >-
  Example code for the state_subscribeStorage WebSocket method. Complete guide
  on how to use state_subscribeStorage WebSocket in GetBlock Web3 documentation.
---

# state\_subscribeStorage - Kusama

Opens a subscription that streams change sets whenever any of the given storage keys change. WebSocket only. Passing an empty key list subscribes to all storage changes (heavy).

{% hint style="warning" %}
This is a WebSocket-only subscription. Connect over `wss://` and unsubscribe with `state_unsubscribeStorage`.
{% endhint %}

## Parameters

| Parameter | Type  | Required | Description                        |
| --------- | ----- | -------- | ---------------------------------- |
| keys      | array | Yes      | Array of hex storage keys to watch |

## Subscribe

{% code overflow="wrap" %}
```bash
wscat -c wss://shared.eu-central-1.getblock.io/<ACCESS-TOKEN>/

# then send:
{
    "jsonrpc": "2.0",
    "id": "getblock.io",
    "method": "state_subscribeStorage",
    "params": [
        [
            "0x26aa394eea5630e07c48ae0c9558cef7b99d880ec681799c0cf30e8886371da9"
        ]
    ]
}
```
{% endcode %}

## Subscription ID

The initial response returns the subscription id used to correlate and cancel the stream:

```json
{
    "jsonrpc": "2.0",
    "id": "getblock.io",
    "result": "Ce9D...e1"
}
```

## Notifications

While subscribed, the node pushes a notification per update:

```json
{
    "jsonrpc": "2.0",
    "method": "state_Storage_notification",
    "params": {
        "subscription": "Ce9D...e1",
        "result": {
            "block": "0x8a1e6d2f0b7c4a9e3d5f1b8c2a6e4d0f9b3c7a5e1d8f2b6c4a0e9d3f7b1c5a2e6",
            "changes": [
                [
                    "0x26aa394eea5630e07c48ae0c9558cef7b99d880ec681799c0cf30e8886371da9",
                    "0x03000000..."
                ]
            ]
        }
    }
}
```

## Notification Fields

| Field   | Type   | Description                                                |
| ------- | ------ | ---------------------------------------------------------- |
| block   | string | Block at which the change occurred                         |
| changes | array  | Array of \[key, value] change pairs; value null if cleared |

## Use Cases

* **Balance Watching**: React to account balance changes
* **Live State**: Keep a cache in sync with on-chain storage
* **Event-Driven UIs**: Update views on state changes

## Error Handling

| Error                     | Message            | Description                                       |
| ------------------------- | ------------------ | ------------------------------------------------- |
| -32603 / Internal error   | Subscription error | The subscription could not be established         |
| 403 / RBAC: access denied | Access denied      | The GetBlock access token is missing or incorrect |
