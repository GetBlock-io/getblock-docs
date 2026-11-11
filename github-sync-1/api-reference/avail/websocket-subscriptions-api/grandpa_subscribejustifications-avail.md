---
description: >-
  Example code for the grandpa_subscribeJustifications WebSocket method.
  Complete guide on how to use grandpa_subscribeJustifications WebSocket in
  GetBlock Web3 documentation.
---

# grandpa\_subscribeJustifications - Avail

Streams GRANDPA finality justifications as blocks are finalized. WebSocket only. A justification is the cryptographic proof that a block is finalized, used by light clients and bridges.

{% hint style="warning" %}
This is a WebSocket-only subscription. Connect over `wss://` and unsubscribe with `grandpa_unsubscribeJustifications`.
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
    "method": "grandpa_subscribeJustifications",
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
    "result": "Gi3H...c5"
}
```

## Notifications

While subscribed, the node pushes a notification per update:

```json
{
    "jsonrpc": "2.0",
    "method": "grandpa_Justifications_notification",
    "params": {
        "subscription": "Gi3H...c5",
        "result": "0x0a1b2c...(SCALE-encoded justification)"
    }
}
```

## Notification Fields

| Field  | Type   | Description                                             |
| ------ | ------ | ------------------------------------------------------- |
| result | string | Hex-encoded GRANDPA justification for a finalized block |

## Use Cases

* **Light Clients**: Verify finality independently
* **Bridges**: Relay finality proofs to another chain
* **Audits**: Record finality justifications

## Error Handling

| Error                     | Message            | Description                                       |
| ------------------------- | ------------------ | ------------------------------------------------- |
| -32603 / Internal error   | Subscription error | The subscription could not be established         |
| 403 / RBAC: access denied | Access denied      | The GetBlock access token is missing or incorrect |
