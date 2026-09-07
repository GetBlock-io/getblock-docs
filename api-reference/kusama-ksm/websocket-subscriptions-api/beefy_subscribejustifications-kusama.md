---
description: >-
  Example code for the beefy_subscribeJustifications WebSocket method. Complete
  guide on how to use beefy_subscribeJustifications WebSocket in GetBlock Web3
  documentation.
---

# beefy\_subscribeJustifications - Kusama

Streams BEEFY finality justifications. WebSocket only. BEEFY is a companion gadget to GRANDPA that produces compact, bridge-friendly finality proofs for light clients on other chains (for example Ethereum).

{% hint style="warning" %}
This is a WebSocket-only subscription. Connect over `wss://` and unsubscribe with `beefy_unsubscribeJustifications`.
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
    "method": "beefy_subscribeJustifications",
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
    "result": "Hj4I...d6"
}
```

## Notifications

While subscribed, the node pushes a notification per update:

```json
{
    "jsonrpc": "2.0",
    "method": "beefy_Justifications_notification",
    "params": {
        "subscription": "Hj4I...d6",
        "result": "0x0455...(SCALE-encoded BEEFY signed commitment)"
    }
}
```

## Notification Fields

| Field  | Type   | Description                         |
| ------ | ------ | ----------------------------------- |
| result | string | Hex-encoded BEEFY signed commitment |

## Use Cases

* **Trustless Bridges**: Consume BEEFY proofs on other chains
* **Light Clients**: Efficient finality verification
* **Interoperability**: Anchor cross-ecosystem messaging

## Error Handling

| Error                     | Message            | Description                                       |
| ------------------------- | ------------------ | ------------------------------------------------- |
| -32603 / Internal error   | Subscription error | The subscription could not be established         |
| 403 / RBAC: access denied | Access denied      | The GetBlock access token is missing or incorrect |
