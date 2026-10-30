# unconfirmed\_txs

Returns transactions currently in the mempool, up to an optional limit.

## Method

```
unconfirmed_txs
```

## Parameters

| Parameter | Type   | Description                              |
| --------- | ------ | ---------------------------------------- |
| limit     | string | Maximum number of transactions to return |

## Example

{% code overflow="wrap" %}
```bash
export STABLE_TM=https://shared.ap-southeast-1.getblock.io/<ACCESS-TOKEN>/

# JSON-RPC over HTTP (POST)
curl -X POST "${STABLE_TM}" -H "Content-Type: application/json" \
  -d '{"jsonrpc": "2.0", "id": "getblock.io", "method": "unconfirmed_txs", "params": {"limit": "30"}}'

# URI over HTTP (GET)
curl "${STABLE_TM}unconfirmed_txs?limit=30"
```
{% endcode %}

## Response

```json
{
    "jsonrpc": "2.0",
    "id": "getblock.io",
    "result": {
        "n_txs": "2",
        "total": "2",
        "total_bytes": "420",
        "txs": [
            "Cg8K..."
        ]
    }
}
```

## Response Fields

| Field  | Type   | Description                         |
| ------ | ------ | ----------------------------------- |
| n\_txs | string | Number of transactions returned     |
| total  | string | Total transactions in the mempool   |
| txs    | array  | Base64-encoded mempool transactions |

## Use Cases

* **Mempool monitoring**: Inspect pending transactions

## Error Handling

| Error                     | Message         | Description                                       |
| ------------------------- | --------------- | ------------------------------------------------- |
| -32600 / Invalid request  | Invalid request | The JSON-RPC envelope was malformed               |
| -32602 / Invalid params   | Invalid params  | A parameter was missing or of the wrong type      |
| 403 / RBAC: access denied | Access denied   | The GetBlock access token is missing or incorrect |
