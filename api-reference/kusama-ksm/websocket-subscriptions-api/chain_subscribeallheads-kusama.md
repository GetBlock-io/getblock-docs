---
description: >-
  Example code for the chain_subscribeAllHeads WebSocket method. Complete guide
  on how to use chain_subscribeAllHeads WebSocket in GetBlock Web3
  documentation.
---

# chain\_subscribeAllHeads - Kusama

Streams every imported block header, including headers from non-canonical forks. WebSocket only. Broader than `chain_subscribeNewHeads`, which follows only the best chain.

{% hint style="warning" %}
This is a WebSocket-only subscription. Connect over `wss://` and unsubscribe with `chain_unsubscribeAllHeads`.
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
    "method": "chain_subscribeAllHeads",
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
    "result": "Eg1F...a3"
}
```

## Notifications

While subscribed, the node pushes a notification per update:

```json
{
    "jsonrpc": "2.0",
    "method": "chain_AllHeads_notification",
    "params": {
        "subscription": "Eg1F...a3",
        "result": {
            "parentHash": "0x...",
            "number": "0x17f7b41",
            "stateRoot": "0x...",
            "extrinsicsRoot": "0x...",
            "digest": {
                "logs": [
                    "0x..."
                ]
            }
        }
    }
}
```

## Notification Fields

| Field      | Type   | Description        |
| ---------- | ------ | ------------------ |
| number     | string | Block number (hex) |
| parentHash | string | Parent block hash  |

## Use Cases

* **Fork Awareness**: Observe competing forks as they arrive
* **Analytics**: Measure fork/uncle rates
* **Indexers**: Track all imported headers

## Error Handling

| Error                     | Message            | Description                                       |
| ------------------------- | ------------------ | ------------------------------------------------- |
| -32603 / Internal error   | Subscription error | The subscription could not be established         |
| 403 / RBAC: access denied | Access denied      | The GetBlock access token is missing or incorrect |
