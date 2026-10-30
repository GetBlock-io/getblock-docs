# num\_unconfirmed\_txs

Returns the count and total size of transactions in the mempool, without the transaction bodies.

## Method

```
num_unconfirmed_txs
```

## Parameters

This method takes no parameters.

## Example

{% code overflow="wrap" %}
```bash
export STABLE_TM=https://shared.ap-southeast-1.getblock.io/<ACCESS-TOKEN>/

# JSON-RPC over HTTP (POST)
curl -X POST "${STABLE_TM}" -H "Content-Type: application/json" \
  -d '{"jsonrpc": "2.0", "id": "getblock.io", "method": "num_unconfirmed_txs", "params": {}}'

# URI over HTTP (GET)
curl "${STABLE_TM}num_unconfirmed_txs"
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
        "total_bytes": "420"
    }
}
```

## Response Fields

| Field        | Type   | Description                        |
| ------------ | ------ | ---------------------------------- |
| n\_txs       | string | Number of mempool transactions     |
| total\_bytes | string | Total size of the mempool in bytes |

## Use Cases

* **Backpressure**: Gauge mempool depth cheaply

## Error Handling

| Error                     | Message         | Description                                       |
| ------------------------- | --------------- | ------------------------------------------------- |
| -32600 / Invalid request  | Invalid request | The JSON-RPC envelope was malformed               |
| -32602 / Invalid params   | Invalid params  | A parameter was missing or of the wrong type      |
| 403 / RBAC: access denied | Access denied   | The GetBlock access token is missing or incorrect |
