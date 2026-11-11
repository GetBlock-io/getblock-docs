---
description: >-
  Example code for the raw table item JSON-RPC method. Complete guide on how to
  use raw table item JSON-RPC in GetBlock Web3 documentation.
---

# raw table item - Movement

Returns a single table item by its raw (BCS-encoded) key — the lower-level counterpart to get table item, used when the key is already serialized.

## Endpoint

```
POST /v1/tables/{table_handle}/raw_item
```

## Path Parameters

| Parameter     | Type   | Description                |
| ------------- | ------ | -------------------------- |
| table\_handle | string | Table handle (0x-prefixed) |

## Query Parameters

| Parameter       | Type   | Description                       |
| --------------- | ------ | --------------------------------- |
| ledger\_version | string | Read at a specific ledger version |

## Example

{% code overflow="wrap" %}
```bash
export MOVEMENT_REST=https://shared.eu-central-1.getblock.io/<ACCESS-TOKEN>/

curl -X POST "${MOVEMENT_REST}v1/tables/0x1234abcd.../raw_item" \
  -H "Content-Type: application/json" \
  -d '{"key_type": "0x1::string::String", "value_type": "u128", "key": "0x0548656c6c6f"}'
```
{% endcode %}

## Response

```json
"1500000000"
```

## Response Fields

| Field  | Type   | Description                                         |
| ------ | ------ | --------------------------------------------------- |
| (root) | varies | The decoded table value for the given raw (BCS) key |

## Use Cases

* **Low-Level Lookups**: Read a table item with a BCS-encoded key

## Error Handling

| Error                     | Message       | Description                                                 |
| ------------------------- | ------------- | ----------------------------------------------------------- |
| 400 / Bad request         | Bad request   | A path, query, or body parameter was invalid                |
| 404 / Not found           | Not found     | The account, block, transaction, or resource does not exist |
| 403 / RBAC: access denied | Access denied | The GetBlock access token is missing or incorrect           |
