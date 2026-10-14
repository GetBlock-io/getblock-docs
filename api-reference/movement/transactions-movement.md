---
description: >-
  Example code for the transactions JSON-RPC method. Complete guide on how to
  use transactions JSON-RPC in GetBlock Web3 documentation.
---

# transactions - Movement

Returns a paginated list of committed transactions, newest-relative to a starting ledger version, with their payloads, events, and effects.

## Endpoint

```http
GET /v1/transactions
```

## Query Parameters

| Parameter | Type   | Description                             |
| --------- | ------ | --------------------------------------- |
| start     | string | Ledger version to start from            |
| limit     | string | Max transactions to return (default 25) |

## Example

{% code overflow="wrap" %}
```bash
export MOVEMENT_REST=https://shared.eu-central-1.getblock.io/<ACCESS-TOKEN>/

curl "${MOVEMENT_REST}v1/transactions?limit=2"
```
{% endcode %}

## Response

```json
[
    {
        "version": "285000000",
        "hash": "0xade...",
        "success": true,
        "vm_status": "Executed successfully",
        "gas_used": "7",
        "sender": "0xabc...",
        "sequence_number": "42",
        "type": "user_transaction"
    }
]
```

## Response Fields

| Field     | Type    | Description                                              |
| --------- | ------- | -------------------------------------------------------- |
| version   | string  | Ledger version                                           |
| hash      | string  | Transaction hash                                         |
| success   | boolean | Whether the transaction succeeded                        |
| gas\_used | string  | Gas units consumed                                       |
| type      | string  | Transaction type (user\_transaction, block\_metadata, …) |

## Use Cases

* **Indexing**: Stream committed transactions
* **Analytics**: Walk transaction history by version

## Error Handling

| Error                     | Message       | Description                                                 |
| ------------------------- | ------------- | ----------------------------------------------------------- |
| 400 / Bad request         | Bad request   | A path, query, or body parameter was invalid                |
| 404 / Not found           | Not found     | The account, block, transaction, or resource does not exist |
| 403 / RBAC: access denied | Access denied | The GetBlock access token is missing or incorrect           |
