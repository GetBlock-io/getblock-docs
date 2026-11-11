---
description: >-
  Example code for the transaction by version JSON-RPC method. Complete guide on
  how to use transaction by version JSON-RPC in GetBlock Web3 documentation.
---

# transaction by version - Movement

Returns a single committed transaction by its ledger version.

## Endpoint

```http
GET /v1/transactions/by_version/{txn_version}
```

## Path Parameters

| Parameter    | Type   | Description                  |
| ------------ | ------ | ---------------------------- |
| txn\_version | string | Ledger (transaction) version |

## Example

{% code overflow="wrap" %}
```bash
export MOVEMENT_REST=https://shared.eu-central-1.getblock.io/<ACCESS-TOKEN>/

curl "${MOVEMENT_REST}v1/transactions/by_version/285000000"
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
    "sender": "0xabc...",
    "type": "user_transaction"
}
```

## Response Fields

| Field   | Type    | Description       |
| ------- | ------- | ----------------- |
| version | string  | Ledger version    |
| hash    | string  | Transaction hash  |
| success | boolean | Execution success |

## Use Cases

* **Deterministic Lookup**: Fetch a transaction by its version

## Error Handling

| Error                     | Message       | Description                                                 |
| ------------------------- | ------------- | ----------------------------------------------------------- |
| 400 / Bad request         | Bad request   | A path, query, or body parameter was invalid                |
| 404 / Not found           | Not found     | The account, block, transaction, or resource does not exist |
| 403 / RBAC: access denied | Access denied | The GetBlock access token is missing or incorrect           |
