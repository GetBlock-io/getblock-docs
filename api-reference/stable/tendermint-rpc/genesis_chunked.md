# genesis\_chunked

Returns the genesis document in chunks, for chains (like Stable) whose genesis is too large to return in one response.

## Method

```
genesis_chunked
```

## Parameters

| Parameter | Type   | Description            |
| --------- | ------ | ---------------------- |
| chunk     | string | Zero-based chunk index |

## Example

{% code overflow="wrap" %}
```bash
export STABLE_TM=https://shared.ap-southeast-1.getblock.io/<ACCESS-TOKEN>/

# JSON-RPC over HTTP (POST)
curl -X POST "${STABLE_TM}" -H "Content-Type: application/json" \
  -d '{"jsonrpc": "2.0", "id": "getblock.io", "method": "genesis_chunked", "params": {"chunk": "0"}}'

# URI over HTTP (GET)
curl "${STABLE_TM}genesis_chunked?chunk=0"
```
{% endcode %}

## Response

```json
{
    "jsonrpc": "2.0",
    "id": "getblock.io",
    "result": {
        "chunk": 0,
        "total": 32,
        "data": "eyJhcHBf..."
    }
}
```

## Response Fields

| Field | Type   | Description                  |
| ----- | ------ | ---------------------------- |
| chunk | number | Index of the returned chunk  |
| total | number | Total number of chunks       |
| data  | string | Base64-encoded genesis chunk |

## Use Cases

* **Large genesis**: Fetch genesis in pieces and concatenate

## Error Handling

| Error                     | Message         | Description                                       |
| ------------------------- | --------------- | ------------------------------------------------- |
| -32600 / Invalid request  | Invalid request | The JSON-RPC envelope was malformed               |
| -32602 / Invalid params   | Invalid params  | A parameter was missing or of the wrong type      |
| 403 / RBAC: access denied | Access denied   | The GetBlock access token is missing or incorrect |
