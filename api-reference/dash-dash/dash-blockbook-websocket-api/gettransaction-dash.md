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
wscat -c wss://shared.eu-central-1.getblock.io/<ACCESS-TOKEN>

# then send:
{
    "id": "getblock.io",
    "method": "getTransaction",
    "params": {
        "txid": "eb0351c64cde2c42bcdf10b11a1ad44bb63631bb6487de70e78b90c2aa57137c"
    }
}
```
{% endcode %}

## Response

```json
{
    "id": "getblock.io",
    "data": {
        "txid": "eb0351c64cde2c42bcdf10b11a1ad44bb63631bb6487de70e78b90c2aa57137c",
        "blockHeight": 2099000,
        "confirmations": 1000,
        "value": "150000000",
        "fees": "22600",
        "vout": [
            {
                "value": "150000000",
                "addresses": [
                    "XjszN1jZJthEoaQDhGthRkaHL9AqaG3Vzw"
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
