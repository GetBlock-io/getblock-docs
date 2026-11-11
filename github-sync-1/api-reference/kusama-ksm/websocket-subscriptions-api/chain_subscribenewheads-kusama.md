---
description: >-
  Example code for the chain_subscribeNewHeads WebSocket method. Complete guide
  on how to use chain_subscribeNewHeads WebSocket in GetBlock Web3
  documentation.
---

# chain\_subscribeNewHeads - Kusama

Opens a subscription that streams the header of every new block as it is imported. WebSocket only. Returns a subscription id, then pushes a notification per new head.

{% hint style="warning" %}
This is a WebSocket-only subscription. Connect over `wss://` and unsubscribe with `chain_unsubscribeNewHeads`.
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
    "method": "chain_subscribeNewHeads",
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
    "result": "Aq7B...c9"
}
```

## Notifications

While subscribed, the node pushes a notification per update:

```json
{
    "jsonrpc": "2.0",
    "method": "chain_NewHeads_notification",
    "params": {
        "subscription": "Aq7B...c9",
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

| Field      | Type   | Description              |
| ---------- | ------ | ------------------------ |
| number     | string | New block number (hex)   |
| parentHash | string | Hash of the parent block |
| stateRoot  | string | State trie root          |

## Use Cases

* **Chain Following**: React to every new block in real time
* **Indexers**: Drive ingestion from the head stream
* **Dashboards**: Live block-height display

## Error Handling

| Error                     | Message            | Description                                       |
| ------------------------- | ------------------ | ------------------------------------------------- |
| -32603 / Internal error   | Subscription error | The subscription could not be established         |
| 403 / RBAC: access denied | Access denied      | The GetBlock access token is missing or incorrect |
