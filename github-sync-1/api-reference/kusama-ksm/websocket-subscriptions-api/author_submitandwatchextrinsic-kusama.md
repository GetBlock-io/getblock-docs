---
description: >-
  Example code for the author_submitAndWatchExtrinsic WebSocket method. Complete
  guide on how to use author_submitAndWatchExtrinsic WebSocket in GetBlock Web3
  documentation.
---

# author\_submitAndWatchExtrinsic - Kusama

Submits a signed extrinsic and opens a subscription that streams its lifecycle status — ready, broadcast, inBlock, finalized, or an error. WebSocket only; the standard way to track a transaction to finality.

{% hint style="warning" %}
This is a WebSocket-only subscription. Connect over `wss://` and unsubscribe with `author_unwatchExtrinsic`.
{% endhint %}

## Parameters

| Parameter | Type   | Required | Description                  |
| --------- | ------ | -------- | ---------------------------- |
| extrinsic | string | Yes      | Hex-encoded signed extrinsic |

## Subscribe

{% code overflow="wrap" %}
```bash
wscat -c wss://shared.eu-central-1.getblock.io/<ACCESS-TOKEN>/

# then send:
{
    "jsonrpc": "2.0",
    "id": "getblock.io",
    "method": "author_submitAndWatchExtrinsic",
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
    "result": "Df0E...f2"
}
```

## Notifications

While subscribed, the node pushes a notification per update:

```json
{
    "jsonrpc": "2.0",
    "method": "author_submitAndWatchExtrinsic_notification",
    "params": {
        "subscription": "Df0E...f2",
        "result": {
            "inBlock": "0x8a1e6d2f0b7c4a9e3d5f1b8c2a6e4d0f9b3c7a5e1d8f2b6c4a0e9d3f7b1c5a2e6"
        }
    }
}
```

## Notification Fields

| Field     | Type   | Description                                                                        |
| --------- | ------ | ---------------------------------------------------------------------------------- |
| status    | string | object \| Lifecycle status: ready, broadcast, inBlock, finalized, dropped, invalid |
| inBlock   | string | Block hash once included                                                           |
| finalized | string | Block hash once finalized                                                          |

## Use Cases

* **Transaction Tracking**: Follow an extrinsic to finalization
* **Wallet UX**: Show live submission status
* **Reliability**: Detect dropped or invalid extrinsics

## Error Handling

| Error                     | Message       | Description                                       |
| ------------------------- | ------------- | ------------------------------------------------- |
| -32603 / Internal error   | Bad extrinsic | The extrinsic is malformed or fails validation    |
| 403 / RBAC: access denied | Access denied | The GetBlock access token is missing or incorrect |
