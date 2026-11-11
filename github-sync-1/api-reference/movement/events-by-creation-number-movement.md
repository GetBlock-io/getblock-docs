---
description: >-
  Example code for the events by creation number JSON-RPC method. Complete guide
  on how to use events by creation number JSON-RPC in GetBlock Web3
  documentation.
---

# events by creation number - Movement

Returns events from an event stream identified by the account address and the stream's creation number, paginated.

## Endpoint

```http
GET /v1/accounts/{address}/events/{creation_number}
```

## Path Parameters

| Parameter        | Type   | Description                                |
| ---------------- | ------ | ------------------------------------------ |
| address          | string | Account that owns the event stream         |
| creation\_number | string | Creation number of the event stream (GUID) |

## Query Parameters

| Parameter | Type   | Description                   |
| --------- | ------ | ----------------------------- |
| start     | string | Sequence number to start from |
| limit     | string | Max events to return          |

## Example

{% code overflow="wrap" %}
```bash
export MOVEMENT_REST=https://shared.eu-central-1.getblock.io/<ACCESS-TOKEN>/

curl "${MOVEMENT_REST}v1/accounts/0x1/events/0?limit=2"
```
{% endcode %}

## Response

```json
[
    {
        "version": "285000000",
        "guid": {
            "creation_number": "0",
            "account_address": "0x1"
        },
        "sequence_number": "0",
        "type": "0x1::coin::DepositEvent",
        "data": {
            "amount": "1000000000"
        }
    }
]
```

## Response Fields

| Field            | Type   | Description                      |
| ---------------- | ------ | -------------------------------- |
| type             | string | Fully-qualified Move event type  |
| data             | object | Decoded event payload            |
| sequence\_number | string | Event sequence within the stream |

## Use Cases

* **Event Processing**: Read events from a known stream
* **Indexing**: Track deposits/withdrawals per account

## Error Handling

| Error                     | Message       | Description                                                 |
| ------------------------- | ------------- | ----------------------------------------------------------- |
| 400 / Bad request         | Bad request   | A path, query, or body parameter was invalid                |
| 404 / Not found           | Not found     | The account, block, transaction, or resource does not exist |
| 403 / RBAC: access denied | Access denied | The GetBlock access token is missing or incorrect           |
