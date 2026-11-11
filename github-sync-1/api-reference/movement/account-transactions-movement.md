---
description: >-
  Example code for the account transactions JSON-RPC method. Complete guide on
  how to use account transactions JSON-RPC in GetBlock Web3 documentation.
---

# account transactions - Movement

Returns the transactions sent by an account (by its sequence numbers), paginated — an account's outbound transaction history.

## Endpoint

```http
GET /v1/accounts/{address}/transactions
```

## Path Parameters

| Parameter | Type   | Description                          |
| --------- | ------ | ------------------------------------ |
| address   | string | Sender account address (0x-prefixed) |

## Query Parameters

| Parameter | Type   | Description                   |
| --------- | ------ | ----------------------------- |
| start     | string | Sequence number to start from |
| limit     | string | Max transactions to return    |

## Example

{% code overflow="wrap" %}
```bash
export MOVEMENT_REST=https://shared.eu-central-1.getblock.io/<ACCESS-TOKEN>/

curl "${MOVEMENT_REST}v1/accounts/0x1/transactions?limit=2"
```
{% endcode %}

## Response

```json
[
    {
        "version": "285000000",
        "hash": "0xade...",
        "sender": "0x1",
        "sequence_number": "42",
        "success": true,
        "type": "user_transaction"
    }
]
```

## Response Fields

| Field  | Type  | Description                                                  |
| ------ | ----- | ------------------------------------------------------------ |
| (root) | array | Transactions sent by the account, ordered by sequence number |

## Use Cases

* **Account History**: List a wallet's outbound transactions
* **Nonce Audit**: Trace sequence-number usage

## Error Handling

| Error                     | Message       | Description                                                 |
| ------------------------- | ------------- | ----------------------------------------------------------- |
| 400 / Bad request         | Bad request   | A path, query, or body parameter was invalid                |
| 404 / Not found           | Not found     | The account, block, transaction, or resource does not exist |
| 403 / RBAC: access denied | Access denied | The GetBlock access token is missing or incorrect           |
