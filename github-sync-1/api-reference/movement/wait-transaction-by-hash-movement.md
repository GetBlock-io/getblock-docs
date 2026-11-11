---
description: >-
  Example code for the wait transaction by hashJSON-RPC method. Complete guide
  on how to use wait transaction by hashJSON-RPC in GetBlock Web3 documentation.
---

# wait transaction by hash - Movement

Returns a transaction by hash, blocking until it is committed (or a timeout). Confirms a just-submitted transaction without client-side polling.

## Endpoint

```http
GET /v1/transactions/wait_by_hash/{txn_hash}
```

## Path Parameters

| Parameter | Type   | Description                    |
| --------- | ------ | ------------------------------ |
| txn\_hash | string | Transaction hash (0x-prefixed) |

## Example

{% code overflow="wrap" %}
```bash
export MOVEMENT_REST=https://shared.eu-central-1.getblock.io/<ACCESS-TOKEN>/

curl "${MOVEMENT_REST}v1/transactions/wait_by_hash/0xade..."
```
{% endcode %}

## Response

```json
{
    "version": "285000000",
    "hash": "0xade...",
    "success": true,
    "vm_status": "Executed successfully",
    "gas_used": "7",
    "type": "user_transaction"
}
```

## Response Fields

| Field      | Type    | Description                       |
| ---------- | ------- | --------------------------------- |
| success    | boolean | Whether the transaction succeeded |
| vm\_status | string  | VM status once committed          |

## Use Cases

* **Confirmation**: Wait for a submitted transaction to commit without polling

## Error Handling

| Error                     | Message       | Description                                                 |
| ------------------------- | ------------- | ----------------------------------------------------------- |
| 400 / Bad request         | Bad request   | A path, query, or body parameter was invalid                |
| 404 / Not found           | Not found     | The account, block, transaction, or resource does not exist |
| 403 / RBAC: access denied | Access denied | The GetBlock access token is missing or incorrect           |
