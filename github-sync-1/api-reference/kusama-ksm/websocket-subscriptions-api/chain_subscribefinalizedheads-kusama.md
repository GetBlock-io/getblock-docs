---
description: >-
  Example code for the chain_subscribeFinalizedHeads WebSocket method. Complete
  guide on how to use chain_subscribeFinalizedHeads WebSocket in GetBlock Web3
  documentation.
---

# chain\_subscribeFinalizedHeads - Kusama

Opens a subscription that streams the header of each newly finalized block. WebSocket only. Finalized blocks are irreversible under GRANDPA.

{% hint style="warning" %}
This is a WebSocket-only subscription. Connect over `wss://` and unsubscribe with `chain_unsubscribeFinalizedHeads`.
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
    "method": "chain_subscribeFinalizedHeads",
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
    "result": "Bd8C...d0"
}
```

## Notifications

While subscribed, the node pushes a notification per update:

```json
{
    "jsonrpc": "2.0",
    "method": "chain_FinalizedHeads_notification",
    "params": {
        "subscription": "Bd8C...d0",
        "result": {
            "parentHash": "0x...",
            "number": "0x17f7b3e",
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

| Field     | Type   | Description                     |
| --------- | ------ | ------------------------------- |
| number    | string | Finalized block number (hex)    |
| stateRoot | string | State trie root at finalization |

## Use Cases

* **Finality Gating**: Act only on finalized blocks
* **Confirmations**: Confirm transactions on finalization
* **Bridges**: Anchor proofs to finalized heads

## Error Handling

| Error                     | Message            | Description                                       |
| ------------------------- | ------------------ | ------------------------------------------------- |
| -32603 / Internal error   | Subscription error | The subscription could not be established         |
| 403 / RBAC: access denied | Access denied      | The GetBlock access token is missing or incorrect |
