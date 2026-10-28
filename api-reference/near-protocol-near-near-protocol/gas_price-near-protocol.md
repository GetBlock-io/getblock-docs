---
description: >-
  Example code for the gas_price JSON-RPC method. Complete guide on how to use
  gas_price JSON-RPC in GetBlock Web3 documentation.
---

# gas\_price - NEAR Protocol

Returns the gas price for a specific block, or the latest price when called with \[null]. The value is in yoctoNEAR per gas unit.

## Method

```
gas_price
```

## Parameters

Takes an empty array `[]` (or `[null]` for the latest block) — see the example.

## Example

{% code overflow="wrap" %}
```bash
export NEAR_RPC=https://shared.eu-central-1.getblock.io/<ACCESS-TOKEN>/

curl -X POST "${NEAR_RPC}" \
  -H "Content-Type: application/json" \
  -d '{"jsonrpc": "2.0", "id": "getblock.io", "method": "gas_price", "params": [null]}'
```
{% endcode %}

## Response

```json
{
    "gas_price": "100000000"
}
```

## Response Fields

| Field      | Type   | Description                            |
| ---------- | ------ | -------------------------------------- |
| gas\_price | string | Gas price in yoctoNEAR per unit of gas |

## Use Cases

* **Fee estimation**: Read the current gas price before building a transaction
* **History**: Query the gas price at a past block height or hash

## Error Handling

| Error                     | Message                  | Description                                                                                             |
| ------------------------- | ------------------------ | ------------------------------------------------------------------------------------------------------- |
| -32700 / Parse error      | Parse error              | The JSON request was malformed                                                                          |
| -32601 / Method not found | Method not found         | The method is unknown or disabled on this node version                                                  |
| HANDLER\_ERROR            | Requested data not found | The account, block, chunk, or transaction does not exist or is garbage-collected (use an archival node) |
| 403 / RBAC: access denied | Access denied            | The GetBlock access token is missing or incorrect                                                       |
