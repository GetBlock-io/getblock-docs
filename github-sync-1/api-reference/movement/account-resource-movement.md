---
description: >-
  Example code for the account resource JSON-RPC method. Complete guide on how
  to use account resource JSON-RPC in GetBlock Web3 documentation.
---

# account resource - Movement

Returns a single Move resource under an account, addressed by its fully-qualified type. Use it to read one resource (such as a specific coin store) directly.

## Endpoint

```http
GET /v1/accounts/{address}/resource/{resource_type}
```

## Path Parameters

| Parameter      | Type   | Description                                                                           |
| -------------- | ------ | ------------------------------------------------------------------------------------- |
| address        | string | Account address (0x-prefixed)                                                         |
| resource\_type | string | Fully-qualified resource type, e.g. 0x1::coin::CoinStore<0x1::aptos\_coin::AptosCoin> |

## Query Parameters

| Parameter       | Type   | Description                       |
| --------------- | ------ | --------------------------------- |
| ledger\_version | string | Read at a specific ledger version |

## Example

{% code overflow="wrap" %}
```bash
export MOVEMENT_REST=https://shared.eu-central-1.getblock.io/<ACCESS-TOKEN>/

curl "${MOVEMENT_REST}v1/accounts/0x1/resource/0x1::coin::CoinStore%3C0x1::aptos_coin::AptosCoin%3E"
```
{% endcode %}

## Response

```json
{
    "type": "0x1::coin::CoinStore<0x1::aptos_coin::AptosCoin>",
    "data": {
        "coin": {
            "value": "1500000000"
        },
        "frozen": false
    }
}
```

## Response Fields

| Field | Type   | Description                 |
| ----- | ------ | --------------------------- |
| type  | string | The resource type requested |
| data  | object | Decoded resource fields     |

## Use Cases

* **Balance Reads**: Read one account's MOVE coin store
* **dApp State**: Fetch a specific Move resource

## Error Handling

| Error                     | Message       | Description                                                 |
| ------------------------- | ------------- | ----------------------------------------------------------- |
| 400 / Bad request         | Bad request   | A path, query, or body parameter was invalid                |
| 404 / Not found           | Not found     | The account, block, transaction, or resource does not exist |
| 403 / RBAC: access denied | Access denied | The GetBlock access token is missing or incorrect           |
