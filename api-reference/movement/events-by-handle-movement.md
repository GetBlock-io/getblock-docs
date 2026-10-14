---
description: >-
  Example code for the events by handle JSON-RPC method. Complete guide on how
  to use events by handle JSON-RPC in GetBlock Web3 documentation.
---

# events by handle - Movement

Returns events from an event handle addressed by the account, the handle's resource struct type, and the field within that struct, paginated.

## Endpoint

```http
GET /v1/accounts/{address}/events/{event_handle}/{field_name}
```

## Path Parameters

| Parameter     | Type   | Description                                             |
| ------------- | ------ | ------------------------------------------------------- |
| address       | string | Account that owns the resource                          |
| event\_handle | string | Fully-qualified struct type that holds the event handle |
| field\_name   | string | Field within the struct that is the EventHandle         |

## Query Parameters

| Parameter | Type   | Description                   |
| --------- | ------ | ----------------------------- |
| start     | string | Sequence number to start from |
| limit     | string | Max events to return          |

## Example

{% code overflow="wrap" %}
```bash
export MOVEMENT_REST=https://shared.eu-central-1.getblock.io/<ACCESS-TOKEN>/

curl "${MOVEMENT_REST}v1/accounts/0x1/events/0x1::coin::CoinStore%3C0x1::aptos_coin::AptosCoin%3E/deposit_events?limit=2"
```
{% endcode %}

## Response

```json
[
    {
        "version": "285000000",
        "guid": {
            "creation_number": "2",
            "account_address": "0x1"
        },
        "sequence_number": "5",
        "type": "0x1::coin::DepositEvent",
        "data": {
            "amount": "1000000000"
        }
    }
]
```

## Response Fields

| Field | Type   | Description           |
| ----- | ------ | --------------------- |
| type  | string | Event type            |
| data  | object | Decoded event payload |

## Use Cases

* **Token Activity**: Read an account's coin deposit/withdraw events
* **dApp Feeds**: Stream events from a specific handle

## Error Handling

| Error                     | Message       | Description                                                 |
| ------------------------- | ------------- | ----------------------------------------------------------- |
| 400 / Bad request         | Bad request   | A path, query, or body parameter was invalid                |
| 404 / Not found           | Not found     | The account, block, transaction, or resource does not exist |
| 403 / RBAC: access denied | Access denied | The GetBlock access token is missing or incorrect           |
