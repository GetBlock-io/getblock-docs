---
description: >-
  Example code for the block_search JSON-RPC method. Complete guide on how to
  use block_search JSON-RPC in GetBlock Web3 documentation.
---

# block\_search - TAC

Searches blocks by an event-attribute query (over begin/end-block events), with pagination and ordering.

## Method

```
block_search
```

## Parameters

| Parameter | Type   | Description      |
| --------- | ------ | ---------------- |
| query     | string | Event query      |
| page      | string | Page number      |
| per\_page | string | Results per page |
| order\_by | string | "asc" or "desc"  |

## Example

{% code overflow="wrap" %}
```bash
export TAC_TM=https://shared.eu-central-1.getblock.io/<ACCESS-TOKEN>/

# JSON-RPC over HTTP (POST)
curl -X POST "${TAC_TM}" -H "Content-Type: application/json" \
  -d '{"jsonrpc": "2.0", "id": "getblock.io", "method": "block_search", "params": {"query": "block.height>5000000", "page": "1", "per_page": "30", "order_by": "desc"}}'

# URI over HTTP (GET)
curl "${TAC_TM}block_search?query=block.height>5000000&page=1&per_page=30&order_by=desc"
```
{% endcode %}

## Response

```json
{
    "jsonrpc": "2.0",
    "id": "getblock.io",
    "result": {
        "blocks": [
            {
                "block_id": {
                    "hash": "A1B2..."
                },
                "block": {
                    "header": {
                        "height": "5123456"
                    }
                }
            }
        ],
        "total_count": "1"
    }
}
```

## Response Fields

| Field        | Type   | Description             |
| ------------ | ------ | ----------------------- |
| blocks       | array  | Matching blocks         |
| total\_count | string | Total number of matches |

## Use Cases

* **Event blocks**: Find blocks emitting a given event

## Error Handling

| Error                     | Message         | Description                                       |
| ------------------------- | --------------- | ------------------------------------------------- |
| -32600 / Invalid request  | Invalid request | The JSON-RPC envelope was malformed               |
| -32602 / Invalid params   | Invalid params  | A parameter was missing or of the wrong type      |
| 403 / RBAC: access denied | Access denied   | The GetBlock access token is missing or incorrect |
