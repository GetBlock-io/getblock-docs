# tx\_search

Searches transactions by an event-attribute query, with pagination and ordering. Useful for finding transactions by sender, recipient, or module events.

## Method

```
tx_search
```

## Parameters

| Parameter | Type   | Description                                     |
| --------- | ------ | ----------------------------------------------- |
| query     | string | Event query, e.g. "message.sender='cosmos1...'" |
| prove     | string | Include Merkle proofs                           |
| page      | string | Page number                                     |
| per\_page | string | Results per page                                |
| order\_by | string | "asc" or "desc"                                 |

## Example

{% code overflow="wrap" %}
```bash
export STABLE_TM=https://shared.ap-southeast-1.getblock.io/<ACCESS-TOKEN>/

# JSON-RPC over HTTP (POST)
curl -X POST "${STABLE_TM}" -H "Content-Type: application/json" \
  -d '{"jsonrpc": "2.0", "id": "getblock.io", "method": "tx_search", "params": {"query": "tx.height=31378844", "prove": "false", "page": "1", "per_page": "30", "order_by": "asc"}}'

# URI over HTTP (GET)
curl "${STABLE_TM}tx_search?query=tx.height=31378844&prove=false&page=1&per_page=30&order_by=asc"
```
{% endcode %}

## Response

```json
{
  "jsonrpc": "2.0",
  "id": "getblock.io",
  "result": {
    "txs": [
      {
        "hash": "9A1B2C...",
        "height": "31378844",
        "tx_result": {
          "code": 0
        }
      }
    ],
    "total_count": "1"
  }
}
```

## Response Fields

| Field        | Type   | Description                        |
| ------------ | ------ | ---------------------------------- |
| txs          | array  | Matching transactions with results |
| total\_count | string | Total number of matches            |

## Use Cases

* **History queries**: Find an address's transactions by event query
* **Module indexing**: Search by module event attributes

## Error Handling

| Error                     | Message         | Description                                       |
| ------------------------- | --------------- | ------------------------------------------------- |
| -32600 / Invalid request  | Invalid request | The JSON-RPC envelope was malformed               |
| -32602 / Invalid params   | Invalid params  | A parameter was missing or of the wrong type      |
| 403 / RBAC: access denied | Access denied   | The GetBlock access token is missing or incorrect |
