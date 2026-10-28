---
description: >-
  Example code for the abci_info JSON-RPC method. Complete guide on how to use
  abci_info JSON-RPC in GetBlock Web3 documentation.
---

# abci\_info - Cosmos

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
export COSMOS_TM=https://shared.eu-central-1.getblock.io/<ACCESS-TOKEN>/

# JSON-RPC over HTTP (POST)
curl -X POST "${COSMOS_TM}" -H "Content-Type: application/json" \
  -d '{"jsonrpc": "2.0", "id": "getblock.io", "method": "abci_info", "params": {}}'

# URI over HTTP (GET)
curl "${COSMOS_TM}abci_info"
```
{% endcode %}

## Response

```json
{
    "jsonrpc": "2.0",
    "id": "getblock.io",
    "result": {
        "response": {
            "data": "gaia",
            "version": "29.0.0",
            "last_block_height": "31378844",
            "last_block_app_hash": "F884..."
        }
    }
}
```

## Response Fields

| Field                           | Type   | Description                |
| ------------------------------- | ------ | -------------------------- |
| response.version                | string | Application (gaia) version |
| response.last\_block\_height    | string | Last committed height      |
| response.last\_block\_app\_hash | string | Last committed app hash    |

## Use Cases

* **App health**: Read the app's last committed height
* **Version check**: Confirm the running gaia version

## Error Handling

| Error                     | Message         | Description                                       |
| ------------------------- | --------------- | ------------------------------------------------- |
| -32600 / Invalid request  | Invalid request | The JSON-RPC envelope was malformed               |
| -32602 / Invalid params   | Invalid params  | A parameter was missing or of the wrong type      |
| 403 / RBAC: access denied | Access denied   | The GetBlock access token is missing or incorrect |
