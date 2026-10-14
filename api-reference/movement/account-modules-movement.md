---
description: >-
  Example code for the account modules JSON-RPC method. Complete guide on how to
  use account modules JSON-RPC in GetBlock Web3 documentation.
---

# account modules - Movement

Returns all Move modules published under an account, each with its bytecode and ABI. Modules define the account's smart-contract code.

## Endpoint

```http
GET /v1/accounts/{address}/modules
```

## Path Parameters

| Parameter | Type   | Description                   |
| --------- | ------ | ----------------------------- |
| address   | string | Account address (0x-prefixed) |

## Query Parameters

| Parameter       | Type   | Description                       |
| --------------- | ------ | --------------------------------- |
| ledger\_version | string | Read at a specific ledger version |
| limit           | string | Max modules to return             |
| start           | string | Pagination cursor                 |

## Example

{% code overflow="wrap" %}
```bash
export MOVEMENT_REST=https://shared.eu-central-1.getblock.io/<ACCESS-TOKEN>/

curl "${MOVEMENT_REST}v1/accounts/0x1/modules"
```
{% endcode %}

## Response

```json
[
    {
        "bytecode": "0xa11ceb0b...",
        "abi": {
            "address": "0x1",
            "name": "coin",
            "exposed_functions": [
                {
                    "name": "balance",
                    "visibility": "public",
                    "is_view": true
                }
            ]
        }
    }
]
```

## Response Fields

| Field    | Type   | Description                               |
| -------- | ------ | ----------------------------------------- |
| bytecode | string | Compiled Move module bytecode (hex)       |
| abi      | object | Module ABI: structs and exposed functions |

## Use Cases

* **Contract Introspection**: Discover a published module's ABI
* **Tooling**: Generate typed bindings from the ABI

## Error Handling

| Error                     | Message       | Description                                                 |
| ------------------------- | ------------- | ----------------------------------------------------------- |
| 400 / Bad request         | Bad request   | A path, query, or body parameter was invalid                |
| 404 / Not found           | Not found     | The account, block, transaction, or resource does not exist |
| 403 / RBAC: access denied | Access denied | The GetBlock access token is missing or incorrect           |
