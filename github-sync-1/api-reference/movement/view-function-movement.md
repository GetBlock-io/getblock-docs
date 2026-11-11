---
description: >-
  Example code for the view function JSON-RPC method. Complete guide on how to
  use view function JSON-RPC in GetBlock Web3 documentation.
---

# view function - Movement

Executes a read-only Move view function against the current (or a historical) ledger state and returns its result, without creating a transaction.

## Endpoint

```http
POST /v1/view
```

## Query Parameters

| Parameter       | Type   | Description                          |
| --------------- | ------ | ------------------------------------ |
| ledger\_version | string | Execute at a specific ledger version |

## Example

{% code overflow="wrap" %}
```bash
export MOVEMENT_REST=https://shared.eu-central-1.getblock.io/<ACCESS-TOKEN>/

curl -X POST "${MOVEMENT_REST}v1/view" \
  -H "Content-Type: application/json" \
  -d '{"function": "0x1::coin::balance", "type_arguments": ["0x1::aptos_coin::AptosCoin"], "arguments": ["0xabc..."]}'
```
{% endcode %}

## Response

```json
[
    "1500000000"
]
```

## Response Fields

| Field  | Type  | Description                       |
| ------ | ----- | --------------------------------- |
| (root) | array | The view function's return values |

## Use Cases

* **Read-Only Calls**: Query a Move view function (e.g. a balance)
* **dApp Reads**: Resolve on-chain state without a transaction

## Error Handling

| Error                     | Message       | Description                                                 |
| ------------------------- | ------------- | ----------------------------------------------------------- |
| 400 / Bad request         | Bad request   | A path, query, or body parameter was invalid                |
| 404 / Not found           | Not found     | The account, block, transaction, or resource does not exist |
| 403 / RBAC: access denied | Access denied | The GetBlock access token is missing or incorrect           |
