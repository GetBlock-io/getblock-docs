---
description: >-
  Example code for the account module JSON-RPC method. Complete guide on how to
  use account module JSON-RPC in GetBlock Web3 documentation.
---

# account module - Movement

Returns a single Move module published under an account, by module name, with its bytecode and ABI.

## Endpoint

```http
GET /v1/accounts/{address}/module/{module_name}
```

## Path Parameters

| Parameter    | Type   | Description                   |
| ------------ | ------ | ----------------------------- |
| address      | string | Account address (0x-prefixed) |
| module\_name | string | Module name, e.g. coin        |

## Query Parameters

| Parameter       | Type   | Description                       |
| --------------- | ------ | --------------------------------- |
| ledger\_version | string | Read at a specific ledger version |

## Example

{% code overflow="wrap" %}
```bash
export MOVEMENT_REST=https://shared.eu-central-1.getblock.io/<ACCESS-TOKEN>/

curl "${MOVEMENT_REST}v1/accounts/0x1/module/coin"
```
{% endcode %}

## Response

```json
{
    "bytecode": "0xa11ceb0b...",
    "abi": {
        "address": "0x1",
        "name": "coin",
        "exposed_functions": [
            {
                "name": "transfer",
                "visibility": "public",
                "is_entry": true
            }
        ]
    }
}
```

## Response Fields

| Field    | Type   | Description                       |
| -------- | ------ | --------------------------------- |
| bytecode | string | Module bytecode (hex)             |
| abi      | object | Module ABI with exposed functions |

## Use Cases

* **Call Building**: Read a module's entry functions and signatures
* **Verification**: Inspect deployed module code

## Error Handling

| Error                     | Message       | Description                                                 |
| ------------------------- | ------------- | ----------------------------------------------------------- |
| 400 / Bad request         | Bad request   | A path, query, or body parameter was invalid                |
| 404 / Not found           | Not found     | The account, block, transaction, or resource does not exist |
| 403 / RBAC: access denied | Access denied | The GetBlock access token is missing or incorrect           |
