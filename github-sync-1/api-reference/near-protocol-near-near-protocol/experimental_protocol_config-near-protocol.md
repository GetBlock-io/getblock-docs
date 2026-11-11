---
description: >-
  Example code for the EXPERIMENTAL_protocol_config JSON-RPC method. Complete
  guide on how to use EXPERIMENTAL_protocol_config JSON-RPC in GetBlock Web3
  documentation.
---

# EXPERIMENTAL\_protocol\_config - NEAR Protocol

Returns the protocol configuration for the latest or a specific block: runtime, gas, and economic parameters currently in force.

## Method

```
EXPERIMENTAL_protocol_config
```

## Parameters

The `params` object carries:

| Parameter | Example   | Description |
| --------- | --------- | ----------- |
| finality  | `"final"` | —           |

## Example

{% code overflow="wrap" %}
```bash
export NEAR_RPC=https://shared.eu-central-1.getblock.io/<ACCESS-TOKEN>/

curl -X POST "${NEAR_RPC}" \
  -H "Content-Type: application/json" \
  -d '{"jsonrpc": "2.0", "id": "getblock.io", "method": "EXPERIMENTAL_protocol_config", "params": {"finality": "final"}}'
```
{% endcode %}

## Response

```json
{
    "protocol_version": 78,
    "chain_id": "mainnet",
    "runtime_config": {
        "storage_amount_per_byte": "10000000000000000000",
        "transaction_costs": {}
    },
    "max_gas_price": "10000000000000000000000"
}
```

## Response Fields

| Field             | Type   | Description                                       |
| ----------------- | ------ | ------------------------------------------------- |
| protocol\_version | number | Active protocol version                           |
| runtime\_config   | object | Runtime, storage, and transaction cost parameters |
| max\_gas\_price   | string | Maximum gas price in yoctoNEAR                    |

## Use Cases

* **Cost modeling**: Read storage and transaction costs to estimate fees
* **Runtime params**: Inspect the active economic configuration

## Error Handling

| Error                     | Message                  | Description                                                                                             |
| ------------------------- | ------------------------ | ------------------------------------------------------------------------------------------------------- |
| -32700 / Parse error      | Parse error              | The JSON request was malformed                                                                          |
| -32601 / Method not found | Method not found         | The method is unknown or disabled on this node version                                                  |
| HANDLER\_ERROR            | Requested data not found | The account, block, chunk, or transaction does not exist or is garbage-collected (use an archival node) |
| 403 / RBAC: access denied | Access denied            | The GetBlock access token is missing or incorrect                                                       |
