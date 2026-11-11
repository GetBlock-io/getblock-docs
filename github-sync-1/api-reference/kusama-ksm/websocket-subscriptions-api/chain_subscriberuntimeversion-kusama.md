---
description: >-
  Example code for the chain_subscribeRuntimeVersion WebSocket method. Complete
  guide on how to use chain_subscribeRuntimeVersion WebSocket in GetBlock Web3
  documentation.
---

# chain\_subscribeRuntimeVersion - Kusama

Streams the runtime version and pushes an update whenever a runtime upgrade takes effect. WebSocket only. Use it to react to on-chain runtime upgrades.

{% hint style="warning" %}
This is a WebSocket-only subscription. Connect over `wss://` and unsubscribe with `chain_unsubscribeRuntimeVersion`.
{% endhint %}

## Parameters

{% hint style="info" %}
This method takes no parameters; send an empty `params` array.
{% endhint %}

## Subscribe

{% code overflow="wrap" %}
```bash
wscat -c wss://shared.eu-central-1.getblock.io/<ACCESS-TOKEN>/

# then send:
{
    "jsonrpc": "2.0",
    "id": "getblock.io",
    "method": "chain_subscribeRuntimeVersion",
    "params": []
}
```
{% endcode %}

## Subscription ID

The initial response returns the subscription id used to correlate and cancel the stream:

```json
{
    "jsonrpc": "2.0",
    "id": "getblock.io",
    "result": "Fh2G...b4"
}
```

## Notifications

While subscribed, the node pushes a notification per update:

```json
{
    "jsonrpc": "2.0",
    "method": "chain_RuntimeVersion_notification",
    "params": {
        "subscription": "Fh2G...b4",
        "result": {
            "specName": "kusama",
            "specVersion": 1003000,
            "transactionVersion": 26
        }
    }
}
```

## Notification Fields

| Field              | Type    | Description                               |
| ------------------ | ------- | ----------------------------------------- |
| specVersion        | integer | New runtime spec version after an upgrade |
| transactionVersion | integer | Extrinsic format version                  |

## Use Cases

* **Upgrade Reaction**: Refresh metadata when the runtime upgrades
* **Signing Safety**: Re-read transaction version after upgrades
* **Monitoring**: Alert on runtime upgrades

## Error Handling

| Error                     | Message            | Description                                       |
| ------------------------- | ------------------ | ------------------------------------------------- |
| -32603 / Internal error   | Subscription error | The subscription could not be established         |
| 403 / RBAC: access denied | Access denied      | The GetBlock access token is missing or incorrect |
