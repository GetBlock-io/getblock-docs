# check\_tx

Runs a CheckTx against the application for a transaction without broadcasting it, returning the validation result.

## Method

```
check_tx
```

## Parameters

| Parameter | Type   | Description                       |
| --------- | ------ | --------------------------------- |
| tx        | string | Base64-encoded signed transaction |

## Example

```bash
export STABLE_TM=https://shared.ap-southeast-1.getblock.io/<ACCESS-TOKEN>/

# JSON-RPC over HTTP (POST)
curl -X POST "${STABLE_TM}" -H "Content-Type: application/json" \
  -d '{"jsonrpc": "2.0", "id": "getblock.io", "method": "check_tx", "params": {"tx": "Cg8KQ..."}}'

# URI over HTTP (GET)
curl "${STABLE_TM}check_tx?tx=Cg8KQ..."
```

## Response

```json
{
    "jsonrpc": "2.0",
    "id": "getblock.io",
    "result": {
        "code": 0,
        "gas_wanted": "200000",
        "gas_used": "84000",
        "log": ""
    }
}
```

## Response Fields

| Field       | Type   | Description                        |
| ----------- | ------ | ---------------------------------- |
| code        | number | Validation result code (0 = valid) |
| gas\_wanted | string | Declared gas limit                 |
| gas\_used   | string | Gas used by CheckTx                |

## Use Cases

* **Dry run**: Validate a transaction before broadcasting

## Error Handling

| Error                     | Message         | Description                                       |
| ------------------------- | --------------- | ------------------------------------------------- |
| -32600 / Invalid request  | Invalid request | The JSON-RPC envelope was malformed               |
| -32602 / Invalid params   | Invalid params  | A parameter was missing or of the wrong type      |
| 403 / RBAC: access denied | Access denied   | The GetBlock access token is missing or incorrect |
