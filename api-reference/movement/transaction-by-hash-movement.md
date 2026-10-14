---
description: >-
  Example code for the transaction by hash JSON-RPC method. Complete guide on
  how to use transaction by hash JSON-RPC in GetBlock Web3 documentation.
---

# transaction by hash - Movement

Returns a single transaction by its hash, including its payload, events, state changes, and VM status. Also resolves pending transactions still in the mempool.

## Endpoint

```http
GET /v1/transactions/by_hash/{txn_hash}
```

## Path Parameters

| Parameter | Type   | Description                    |
| --------- | ------ | ------------------------------ |
| txn\_hash | string | Transaction hash (0x-prefixed) |

## Example

{% code overflow="wrap" %}
```bash
export MOVEMENT_REST=https://shared.eu-central-1.getblock.io/<ACCESS-TOKEN>/

curl "${MOVEMENT_REST}v1/transactions/by_hash/0xade..."
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
    "gas_unit_price": "100",
    "sender": "0xabc...",
    "sequence_number": "42",
    "payload": {
        "type": "entry_function_payload",
        "function": "0x1::coin::transfer",
        "arguments": [
            "0xdef...",
            "1000000000"
        ]
    },
    "events": []
}
```

## Response Fields

| Field      | Type    | Description                                         |
| ---------- | ------- | --------------------------------------------------- |
| success    | boolean | Whether the transaction succeeded                   |
| vm\_status | string  | Human-readable VM status                            |
| gas\_used  | string  | Gas consumed                                        |
| payload    | object  | The transaction payload (entry function, script, …) |
| events     | array   | Emitted events                                      |

## Use Cases

* **Receipt Lookup**: Confirm a transaction and read its effects
* **Pending Tracking**: Resolve a transaction still in the mempool

## Error Handling

| Error                     | Message       | Description                                                 |
| ------------------------- | ------------- | ----------------------------------------------------------- |
| 400 / Bad request         | Bad request   | A path, query, or body parameter was invalid                |
| 404 / Not found           | Not found     | The account, block, transaction, or resource does not exist |
| 403 / RBAC: access denied | Access denied | The GetBlock access token is missing or incorrect           |
