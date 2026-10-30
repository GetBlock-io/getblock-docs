# header\_by\_hash

Returns a block header by its hash.

## Method

```
header_by_hash
```

## Parameters

| Parameter | Type   | Description |
| --------- | ------ | ----------- |
| hash      | string | Block hash  |

## Example

{% code overflow="wrap" %}
```bash
export STABLE_TM=https://shared.ap-southeast-1.getblock.io/<ACCESS-TOKEN>/

# JSON-RPC over HTTP (POST)
curl -X POST "${STABLE_TM}" -H "Content-Type: application/json" \
  -d '{"jsonrpc": "2.0", "id": "getblock.io", "method": "header_by_hash", "params": {"hash": "0xA1B2C3..."}}'

# URI over HTTP (GET)
curl "${STABLE_TM}header_by_hash?hash=0xA1B2C3..."
```
{% endcode %}

## Response

```json
{
    "jsonrpc": "2.0",
    "id": "getblock.io",
    "result": {
        "header": {
            "height": "31378844"
        }
    }
}
```

## Response Fields

| Field  | Type   | Description                         |
| ------ | ------ | ----------------------------------- |
| header | object | The block header for the given hash |

## Use Cases

* **Hash lookup**: Resolve a header when you have its block hash

## Error Handling

| Error                     | Message         | Description                                       |
| ------------------------- | --------------- | ------------------------------------------------- |
| -32600 / Invalid request  | Invalid request | The JSON-RPC envelope was malformed               |
| -32602 / Invalid params   | Invalid params  | A parameter was missing or of the wrong type      |
| 403 / RBAC: access denied | Access denied   | The GetBlock access token is missing or incorrect |
