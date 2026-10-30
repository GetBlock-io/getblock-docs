# abci\_info

Returns information about the ABCI application: its version and the last block height and app hash it has committed.

## Method

```
abci_info
```

## Parameters

This method takes no parameters.

## Example

{% code overflow="wrap" %}
```bash
export STABLE_TM=https://shared.ap-southeast-1.getblock.io/<ACCESS-TOKEN>/

# JSON-RPC over HTTP (POST)
curl -X POST "${STABLE_TM}" -H "Content-Type: application/json" \
  -d '{"jsonrpc": "2.0", "id": "getblock.io", "method": "abci_info", "params": {}}'

# URI over HTTP (GET)
curl "${STABLE_TM}abci_info"
```
{% endcode %}

## Response

```json
{
    "jsonrpc": "2.0",
    "id": "getblock.io",
    "result": {
        "response": {
            "data": "stabled",
            "version": "29.0.0",
            "last_block_height": "31378844",
            "last_block_app_hash": "F884..."
        }
    }
}
```

## Response Fields

| Field                           | Type   | Description                   |
| ------------------------------- | ------ | ----------------------------- |
| response.version                | string | Application (stabled) version |
| response.last\_block\_height    | string | Last committed height         |
| response.last\_block\_app\_hash | string | Last committed app hash       |

## Use Cases

* **App health**: Read the app's last committed height
* **Version check**: Confirm the running stabled version

## Error Handling

| Error                     | Message         | Description                                       |
| ------------------------- | --------------- | ------------------------------------------------- |
| -32600 / Invalid request  | Invalid request | The JSON-RPC envelope was malformed               |
| -32602 / Invalid params   | Invalid params  | A parameter was missing or of the wrong type      |
| 403 / RBAC: access denied | Access denied   | The GetBlock access token is missing or incorrect |
