---
description: >-
  Example code for the net_info JSON-RPC method. Complete guide on how to use
  net_info JSON-RPC in GetBlock Web3 documentation.
---

# net\_info - Cosmos

Returns peer-to-peer network information: the number of connected peers, listeners, and per-peer connection details.

## Method

```
net_info
```

## Parameters

This method takes no parameters.

## Example

{% code overflow="wrap" %}
```bash
export COSMOS_TM=https://shared.eu-central-1.getblock.io/<ACCESS-TOKEN>/

# JSON-RPC over HTTP (POST)
curl -X POST "${COSMOS_TM}" -H "Content-Type: application/json" \
  -d '{"jsonrpc": "2.0", "id": "getblock.io", "method": "net_info", "params": {}}'

# URI over HTTP (GET)
curl "${COSMOS_TM}net_info"
```
{% endcode %}

## Response

```json
{
    "jsonrpc": "2.0",
    "id": "getblock.io",
    "result": {
        "listening": true,
        "listeners": [
            "Listener(@)"
        ],
        "n_peers": "24",
        "peers": []
    }
}
```

## Response Fields

| Field     | Type    | Description                                       |
| --------- | ------- | ------------------------------------------------- |
| listening | boolean | Whether the node is listening for P2P connections |
| n\_peers  | string  | Number of connected peers                         |
| peers     | array   | Per-peer connection details                       |

## Use Cases

* **Connectivity**: Confirm the node has healthy peering

## Error Handling

| Error                     | Message         | Description                                       |
| ------------------------- | --------------- | ------------------------------------------------- |
| -32600 / Invalid request  | Invalid request | The JSON-RPC envelope was malformed               |
| -32602 / Invalid params   | Invalid params  | A parameter was missing or of the wrong type      |
| 403 / RBAC: access denied | Access denied   | The GetBlock access token is missing or incorrect |
