# broadcast\_tx\_async

Broadcasts a transaction and returns immediately, without running CheckTx or waiting for inclusion. The transaction may still be rejected.

## Method

```
broadcast_tx_async
```

## Parameters

| Parameter | Type   | Description                       |
| --------- | ------ | --------------------------------- |
| tx        | string | Base64-encoded signed transaction |

## Example

{% code overflow="wrap" %}
```bash
export STABLE_TM=https://shared.ap-southeast-1.getblock.io/<ACCESS-TOKEN>/

# JSON-RPC over HTTP (POST)
curl -X POST "${STABLE_TM}" -H "Content-Type: application/json" \
  -d '{"jsonrpc": "2.0", "id": "getblock.io", "method": "broadcast_tx_async", "params": {"tx": "Cg8KQ..."}}'

# URI over HTTP (GET)
curl "${STABLE_TM}broadcast_tx_async?tx=Cg8KQ..."
```
{% endcode %}

## Response

```json
{
    "jsonrpc": "2.0",
    "id": "getblock.io",
    "result": {
        "code": 0,
        "hash": "9A1B2C..."
    }
}
```

## Response Fields

| Field | Type   | Description                          |
| ----- | ------ | ------------------------------------ |
| code  | number | Always 0 (the node did not validate) |
| hash  | string | The transaction hash                 |

## Use Cases

* **Fire-and-forget**: Submit without waiting for validation

## Error Handling

| Error                     | Message         | Description                                       |
| ------------------------- | --------------- | ------------------------------------------------- |
| -32600 / Invalid request  | Invalid request | The JSON-RPC envelope was malformed               |
| -32602 / Invalid params   | Invalid params  | A parameter was missing or of the wrong type      |
| 403 / RBAC: access denied | Access denied   | The GetBlock access token is missing or incorrect |
