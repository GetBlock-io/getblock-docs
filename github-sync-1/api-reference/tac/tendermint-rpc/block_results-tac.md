---
description: >-
  Example code for the block_results JSON-RPC method. Complete guide on how to
  use block_results JSON-RPC in GetBlock Web3 documentation.
---

# block\_results - TAC

Returns the results of all transactions in a block: per-tx execution results, and the block's begin/end-block events and validator/consensus-param updates.

## Method

```
block_results
```

## Parameters

| Parameter | Type   | Description                    |
| --------- | ------ | ------------------------------ |
| height    | string | Block height (omit for latest) |

## Example

{% code overflow="wrap" %}
```bash
export TAC_TM=https://shared.eu-central-1.getblock.io/<ACCESS-TOKEN>/

# JSON-RPC over HTTP (POST)
curl -X POST "${TAC_TM}" -H "Content-Type: application/json" \
  -d '{"jsonrpc": "2.0", "id": "getblock.io", "method": "block_results", "params": {"height": "5123456"}}'

# URI over HTTP (GET)
curl "${TAC_TM}block_results?height=5123456"
```
{% endcode %}

## Response

```json
{
    "jsonrpc": "2.0",
    "id": "getblock.io",
    "result": {
        "height": "5123456",
        "txs_results": [
            {
                "code": 0,
                "gas_used": "42000",
                "events": []
            }
        ],
        "finalize_block_events": []
    }
}
```

## Response Fields

| Field                   | Type   | Description                                    |
| ----------------------- | ------ | ---------------------------------------------- |
| height                  | string | The block height                               |
| txs\_results            | array  | Per-transaction results: code, gas, and events |
| finalize\_block\_events | array  | Block-level events emitted during finalization |

## Use Cases

* **Event indexing**: Read transaction and block events for indexing
* **Receipt data**: Inspect per-tx result codes and gas usage

## Error Handling

| Error                     | Message         | Description                                       |
| ------------------------- | --------------- | ------------------------------------------------- |
| -32600 / Invalid request  | Invalid request | The JSON-RPC envelope was malformed               |
| -32602 / Invalid params   | Invalid params  | A parameter was missing or of the wrong type      |
| 403 / RBAC: access denied | Access denied   | The GetBlock access token is missing or incorrect |
