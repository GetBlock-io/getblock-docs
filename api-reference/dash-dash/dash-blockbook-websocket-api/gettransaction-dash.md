---
description: >-
  Example code for the getTransaction WebSocket method. Complete guide on how to
  use the getTransaction WebSocket method in the GetBlock Web3 documentation.
---

# getTransaction - Dash

Returns a normalized transaction by txid over WebSocket, with resolved input and output addresses and values.

## Parameters

| Parameter | Type   | Required | Description    |
| --------- | ------ | -------- | -------------- |
| txid      | string | Yes      | Transaction id |

## Message

{% code overflow="wrap" %}
```bash
wscat -c wss://shared.eu-central-1.getblock.io/<ACCESS-TOKEN>/websocket

# then send:
{
    "id": "getblock.io",
    "method": "getTransaction",
    "params": {
        "txid": "3a1f9c2e7b4d8a05f6c1e3d9b2a4c6e8f0d1b3a5c7e9f2d4b6a8c0e1f3d5b7a9c"
    }
}
```
{% endcode %}

## Response

```json
{
    "id": "getblock.io",
    "data": {
        "txid": "3a1f9c2e7b4d8a05f6c1e3d9b2a4c6e8f0d1b3a5c7e9f2d4b6a8c0e1f3d5b7a9c",
        "blockHeight": 2099000,
        "confirmations": 1000,
        "value": "150000000",
        "fees": "22600",
        "vout": [
            {
                "value": "150000000",
                "addresses": [
                    "XvKqL8m3nP7rT2wZ5aB9cD4eF6gH1jK0mN"
                ]
            }
        ]
    }
}
```

## Response Fields

| Field | Type   | Description                     |
| ----- | ------ | ------------------------------- |
| value | string | Total output value in duffs     |
| fees  | string | Fee in duffs                    |
| vout  | array  | Outputs with resolved addresses |

## Use Cases

* **Transaction Reads**: Fetch a tx over WS
* **Payment Confirmation**: Check confirmations
* **Explorers**: Populate tx views

## Error Handling

| Error                     | Message       | Description                                       |
| ------------------------- | ------------- | ------------------------------------------------- |
| error                     | Not found     | No transaction matches the txid                   |
| 403 / RBAC: access denied | Access denied | The GetBlock access token is missing or incorrect |
