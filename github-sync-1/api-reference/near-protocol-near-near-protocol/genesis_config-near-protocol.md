---
description: >-
  Example code for the genesis_config JSON-RPC method. Complete guide on how to
  use genesis_config JSON-RPC in GetBlock Web3 documentation.
---

# genesis\_config - NEAR Protocol

Returns the network's genesis configuration: the parameters the chain was launched with.

## Method

```
genesis_config
```

## Parameters

See the example below for the request shape.

## Example

{% code overflow="wrap" %}
```bash
export NEAR_RPC=https://shared.eu-central-1.getblock.io/<ACCESS-TOKEN>/

curl -X POST "${NEAR_RPC}" \
  -H "Content-Type: application/json" \
  -d '{"jsonrpc": "2.0", "id": "getblock.io", "method": "genesis_config", "params": {}}'
```
{% endcode %}

## Response

```json
{
    "protocol_version": 29,
    "chain_id": "mainnet",
    "genesis_height": 9820210,
    "num_block_producer_seats": 100,
    "max_gas_price": "10000000000000000000000",
    "total_supply": "1000000000000000000000000000000000"
}
```

## Response Fields

| Field                       | Type   | Description                          |
| --------------------------- | ------ | ------------------------------------ |
| chain\_id                   | string | Network identifier                   |
| genesis\_height             | number | Block height of genesis              |
| total\_supply               | string | Initial total supply in yoctoNEAR    |
| num\_block\_producer\_seats | number | Block-producer seat count at genesis |

## Use Cases

* **Chain bootstrap**: Read genesis parameters
* **Verification**: Confirm the chain\_id and genesis height

## Error Handling

| Error                     | Message                  | Description                                                                                             |
| ------------------------- | ------------------------ | ------------------------------------------------------------------------------------------------------- |
| -32700 / Parse error      | Parse error              | The JSON request was malformed                                                                          |
| -32601 / Method not found | Method not found         | The method is unknown or disabled on this node version                                                  |
| HANDLER\_ERROR            | Requested data not found | The account, block, chunk, or transaction does not exist or is garbage-collected (use an archival node) |
| 403 / RBAC: access denied | Access denied            | The GetBlock access token is missing or incorrect                                                       |
