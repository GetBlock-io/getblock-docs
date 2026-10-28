---
description: >-
  Example code for the broadcast_tx_async JSON-RPC method. Complete guide on how
  to use broadcast_tx_async JSON-RPC in GetBlock Web3 documentation.
---

# broadcast\_tx\_async - NEAR Protocol

Broadcasts a signed transaction and returns its hash immediately, without waiting for execution. Equivalent to send\_tx with wait\_until NONE.

## Method

```
broadcast_tx_async
```

## Parameters

A positional array; the first element is shown in the example below.

## Example

{% code overflow="wrap" %}
```bash
export NEAR_RPC=https://shared.eu-central-1.getblock.io/<ACCESS-TOKEN>/

curl -X POST "${NEAR_RPC}" \
  -H "Content-Type: application/json" \
  -d '{"jsonrpc": "2.0", "id": "getblock.io", "method": "broadcast_tx_async", "params": ["DwAAAG5lYXJrYXQudGVzdG5ldA..."]}'
```
{% endcode %}

## Response

```json
"B2dEThkFLWv8y5xjNUSW96NU7mKGyRk8sLh1G8sB8qL9"
```

## Response Fields

| Field   | Type   | Description                                  |
| ------- | ------ | -------------------------------------------- |
| (value) | string | The transaction hash; poll tx for its status |

## Use Cases

* **Fire-and-forget**: Submit without waiting, then poll tx
* **Throughput**: Maximize submission rate in batch jobs

## Error Handling

| Error                     | Message                  | Description                                                                                             |
| ------------------------- | ------------------------ | ------------------------------------------------------------------------------------------------------- |
| -32700 / Parse error      | Parse error              | The JSON request was malformed                                                                          |
| -32601 / Method not found | Method not found         | The method is unknown or disabled on this node version                                                  |
| HANDLER\_ERROR            | Requested data not found | The account, block, chunk, or transaction does not exist or is garbage-collected (use an archival node) |
| 403 / RBAC: access denied | Access denied            | The GetBlock access token is missing or incorrect                                                       |
