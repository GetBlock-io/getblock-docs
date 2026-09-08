---
description: >-
  Example code for the sendTransaction WebSocket method. Complete guide on how
  to use the sendTransaction WebSocket method in the GetBlock Web3
  documentation.
---

# sendTransaction - Dash

Broadcasts a signed, serialized transaction over WebSocket and returns its txid.

## Parameters

| Parameter | Type   | Required | Description                       |
| --------- | ------ | -------- | --------------------------------- |
| hex       | string | Yes      | Signed serialized transaction hex |

## Message

{% code overflow="wrap" %}
```bash
wscat -c wss://shared.eu-central-1.getblock.io/<ACCESS-TOKEN>/websocket

# then send:
{
    "id": "getblock.io",
    "method": "sendTransaction",
    "params": {
        "hex": "0300000001..."
    }
}
```
{% endcode %}

## Response

```json
{
    "id": "getblock.io",
    "data": {
        "result": "3a1f9c2e7b4d8a05f6c1e3d9b2a4c6e8f0d1b3a5c7e9f2d4b6a8c0e1f3d5b7a9c"
    }
}
```

## Response Fields

| Field  | Type   | Description                                 |
| ------ | ------ | ------------------------------------------- |
| result | string | Transaction id of the broadcast transaction |

## Use Cases

* **Transaction Submission**: Broadcast over a live WS connection
* **Wallet Backends**: Submit and then subscribe for confirmation
* **Low Latency**: Send without opening a new HTTP request

## Error Handling

| Error                     | Message             | Description                                       |
| ------------------------- | ------------------- | ------------------------------------------------- |
| error                     | Invalid transaction | The transaction failed validation                 |
| 403 / RBAC: access denied | Access denied       | The GetBlock access token is missing or incorrect |
