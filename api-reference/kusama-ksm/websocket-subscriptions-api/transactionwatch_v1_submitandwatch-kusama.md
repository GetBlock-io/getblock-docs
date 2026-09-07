---
description: >-
  Example code for the transactionWatch_v1_submitAndWatch WebSocket method.
  Complete guide on how to use transactionWatch_v1_submitAndWatch WebSocket in
  GetBlock Web3 documentation.
---

# transactionWatch\_v1\_submitAndWatch - Kusama

Submits a signed transaction and streams its lifecycle via the new JSON-RPC specification: validated, broadcasted, bestChainBlockIncluded, finalized, or an error/drop event. WebSocket only. The modern replacement for author\_submitAndWatchExtrinsic.

{% hint style="warning" %}
This is a WebSocket-only subscription. Connect over `wss://` and unsubscribe with `transactionWatch_v1_unwatch`.
{% endhint %}

## Parameters

| Parameter   | Type   | Required | Description                    |
| ----------- | ------ | -------- | ------------------------------ |
| transaction | string | Yes      | Hex-encoded signed transaction |

## Subscribe

{% code overflow="wrap" %}
```bash
wscat -c wss://shared.eu-central-1.getblock.io/<ACCESS-TOKEN>/

# then send:
{
    "jsonrpc": "2.0",
    "id": "getblock.io",
    "method": "transactionWatch_v1_submitAndWatch",
    "params": [
        "0x4d028400d43593c715fdd31c61141abd04a99fd6822c8558854ccde39a5684e7a56da27d..."
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
    "result": "Ik5J...e7"
}
```

## Notifications

While subscribed, the node pushes a notification per update:

```json
{
    "jsonrpc": "2.0",
    "method": "transactionWatch_v1_submitAndWatch_notification",
    "params": {
        "subscription": "Ik5J...e7",
        "result": {
            "event": "bestChainBlockIncluded",
            "block": {
                "hash": "0x8a1e6d2f0b7c4a9e3d5f1b8c2a6e4d0f9b3c7a5e1d8f2b6c4a0e9d3f7b1c5a2e6",
                "index": 2
            }
        }
    }
}
```

## Notification Fields

| Field | Type   | Description                                                                                         |
| ----- | ------ | --------------------------------------------------------------------------------------------------- |
| event | string | Lifecycle event: validated, broadcasted, bestChainBlockIncluded, finalized, error, dropped, invalid |
| block | object | Block hash and index once included/finalized                                                        |

## Use Cases

* **Transaction Tracking**: Follow a transaction to finalization (new spec)
* **Wallet UX**: Show precise lifecycle status
* **Reliability**: Detect drops and invalid transactions

## Error Handling

| Error                     | Message             | Description                                       |
| ------------------------- | ------------------- | ------------------------------------------------- |
| -32603 / Internal error   | Invalid transaction | The transaction is malformed or fails validation  |
| 403 / RBAC: access denied | Access denied       | The GetBlock access token is missing or incorrect |
