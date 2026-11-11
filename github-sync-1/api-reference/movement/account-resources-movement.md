---
description: >-
  Example code for the account resources JSON-RPC method. Complete guide on how
  to use account resources JSON-RPC in GetBlock Web3 documentation.
---

# account resources - Movement

Returns all Move resources stored under an account, each with its fully-qualified type and decoded data — including the coin store that holds the account's MOVE balance.

## Endpoint

```http
GET /v1/accounts/{address}/resources
```

## Path Parameters

| Parameter | Type   | Description                   |
| --------- | ------ | ----------------------------- |
| address   | string | Account address (0x-prefixed) |

## Query Parameters

| Parameter       | Type   | Description                       |
| --------------- | ------ | --------------------------------- |
| ledger\_version | string | Read at a specific ledger version |
| limit           | string | Max resources to return           |
| start           | string | Pagination cursor                 |

## Example

{% code overflow="wrap" %}
```bash
export MOVEMENT_REST=https://shared.eu-central-1.getblock.io/<ACCESS-TOKEN>/

curl "${MOVEMENT_REST}v1/accounts/0x1/resources"
```
{% endcode %}

## Response

```json
[
  {
    "type": "0x1::coin::CoinStore<0x1::aptos_coin::AptosCoin>",
    "data": {
      "coin": {
        "value": "1500000000"
      },
      "frozen": false
    }
  },
  {
    "type": "0x1::account::Account",
    "data": {
      "sequence_number": "42"
    }
  }
]
```

## Response Fields

| Field | Type   | Description                        |
| ----- | ------ | ---------------------------------- |
| type  | string | Fully-qualified Move resource type |
| data  | object | Decoded resource fields            |

## Use Cases

* **Balance Reads**: Read the CoinStore resource for a MOVE balance
* **State Inspection**: Enumerate all resources under an account

## Error Handling

| Error                     | Message       | Description                                                 |
| ------------------------- | ------------- | ----------------------------------------------------------- |
| 400 / Bad request         | Bad request   | A path, query, or body parameter was invalid                |
| 404 / Not found           | Not found     | The account, block, transaction, or resource does not exist |
| 403 / RBAC: access denied | Access denied | The GetBlock access token is missing or incorrect           |
