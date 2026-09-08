---
description: >-
  Example code for the estimateFee WebSocket method. Complete guide on how to
  use the estimateFee WebSocket method in the GetBlock Web3 documentation.
---

# estimateFee - Dash

Returns fee-rate estimates for one or more confirmation-block targets over WebSocket.

## Parameters

| Parameter | Type  | Required | Description                                     |
| --------- | ----- | -------- | ----------------------------------------------- |
| blocks    | array | Yes      | Array of confirmation targets, e.g. \[1, 6, 12] |

## Message

{% code overflow="wrap" %}
```bash
wscat -c wss://shared.eu-central-1.getblock.io/<ACCESS-TOKEN>/websocket

# then send:
{
    "id": "getblock.io",
    "method": "estimateFee",
    "params": {
        "blocks": [
            1,
            6,
            12
        ]
    }
}
```
{% endcode %}

## Response

```json
{
    "id": "getblock.io",
    "data": [
        {
            "feePerUnit": "0.00002500"
        },
        {
            "feePerUnit": "0.00001500"
        },
        {
            "feePerUnit": "0.00001000"
        }
    ]
}
```

## Response Fields

| Field      | Type   | Description                              |
| ---------- | ------ | ---------------------------------------- |
| feePerUnit | string | Estimated fee rate per target in DASH/kB |

## Use Cases

* **Fee Selection**: Fetch several targets at once
* **Wallets**: Offer fee tiers
* **Batching**: Pick economical fees

## Error Handling

| Error                     | Message       | Description                                       |
| ------------------------- | ------------- | ------------------------------------------------- |
| error                     | Bad request   | The targets are invalid                           |
| 403 / RBAC: access denied | Access denied | The GetBlock access token is missing or incorrect |
