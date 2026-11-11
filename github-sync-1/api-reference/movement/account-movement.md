---
description: >-
  Example code for the account JSON-RPC method. Complete guide on how to use
  account JSON-RPC in GetBlock Web3 documentation.
---

# account - Movement

Returns the core on-chain data of an account: its current sequence number (used as the nonce for the next transaction) and its authentication key.

## Endpoint

```http
GET /v1/accounts/{address}
```

## Path Parameters

| Parameter | Type   | Description                                |
| --------- | ------ | ------------------------------------------ |
| address   | string | Account address (0x-prefixed, 32-byte hex) |

## Query Parameters

| Parameter       | Type   | Description                                   |
| --------------- | ------ | --------------------------------------------- |
| ledger\_version | string | Read the account at a specific ledger version |

## Example

{% code overflow="wrap" %}
```bash
export MOVEMENT_REST=https://shared.eu-central-1.getblock.io/<ACCESS-TOKEN>/

curl "${MOVEMENT_REST}v1/accounts/0x1"
```
{% endcode %}

## Response

```json
{
    "sequence_number": "42",
    "authentication_key": "0x0000000000000000000000000000000000000000000000000000000000000001"
}
```

## Response Fields

| Field               | Type   | Description                              |
| ------------------- | ------ | ---------------------------------------- |
| sequence\_number    | string | Next transaction sequence number (nonce) |
| authentication\_key | string | Account authentication key               |

## Use Cases

* **Transaction Building**: Read the sequence number before submitting
* **Account Checks**: Confirm an account exists on chain

## Error Handling

| Error                     | Message       | Description                                                 |
| ------------------------- | ------------- | ----------------------------------------------------------- |
| 400 / Bad request         | Bad request   | A path, query, or body parameter was invalid                |
| 404 / Not found           | Not found     | The account, block, transaction, or resource does not exist |
| 403 / RBAC: access denied | Access denied | The GetBlock access token is missing or incorrect           |
