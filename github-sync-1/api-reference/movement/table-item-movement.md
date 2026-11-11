---
description: >-
  Example code for the table item JSON-RPC method. Complete guide on how to use
  table item JSON-RPC in GetBlock Web3 documentation.
---

# table item - Movement

Returns a single item from a Move table by its key. The body specifies the key type, value type, and the key value. Tables back large key-value stores in Move.

## Endpoint

```http
POST /v1/tables/{table_handle}/item
```

## Path Parameters

| Parameter     | Type   | Description                                                   |
| ------------- | ------ | ------------------------------------------------------------- |
| table\_handle | string | Table handle (0x-prefixed), obtained from the owning resource |

## Query Parameters

| Parameter       | Type   | Description                       |
| --------------- | ------ | --------------------------------- |
| ledger\_version | string | Read at a specific ledger version |

## Example

{% code overflow="wrap" %}
```bash
export MOVEMENT_REST=https://shared.eu-central-1.getblock.io/<ACCESS-TOKEN>/

curl -X POST "${MOVEMENT_REST}v1/tables/0x1234abcd.../item" \
  -H "Content-Type: application/json" \
  -d '{"key_type": "address", "value_type": "u128", "key": "0xabc..."}'
```
{% endcode %}

## Response

```json
"1500000000"
```

## Response Fields

| Field  | Type   | Description                                                |
| ------ | ------ | ---------------------------------------------------------- |
| (root) | varies | The table value for the given key, decoded per value\_type |

## Use Cases

* **Collections**: Read one entry of an on-chain table by key
* **dApp State**: Look up table-backed records

## Error Handling

| Error                     | Message       | Description                                                 |
| ------------------------- | ------------- | ----------------------------------------------------------- |
| 400 / Bad request         | Bad request   | A path, query, or body parameter was invalid                |
| 404 / Not found           | Not found     | The account, block, transaction, or resource does not exist |
| 403 / RBAC: access denied | Access denied | The GetBlock access token is missing or incorrect           |
