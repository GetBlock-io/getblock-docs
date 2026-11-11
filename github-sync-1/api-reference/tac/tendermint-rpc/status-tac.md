---
description: >-
  Example code for the status JSON-RPC method. Complete guide on how to use
  status JSON-RPC in GetBlock Web3 documentation.
---

# status - TAC

Returns node information: protocol and software version, network (chain id), latest block hash, app hash, block height and time, and the node's validator info and sync state.

## Method

```
status
```

## Parameters

This method takes no parameters.

## Example

{% code overflow="wrap" %}
```bash
export TAC_TM=https://shared.eu-central-1.getblock.io/<ACCESS-TOKEN>/

# JSON-RPC over HTTP (POST)
curl -X POST "${TAC_TM}" -H "Content-Type: application/json" \
  -d '{"jsonrpc": "2.0", "id": "getblock.io", "method": "status", "params": {}}'

# URI over HTTP (GET)
curl "${TAC_TM}status"
```
{% endcode %}

## Response

```json
{
    "jsonrpc": "2.0",
    "id": "getblock.io",
    "result": {
        "node_info": {
            "network": "tacchain_2390-1",
            "version": "0.38.19",
            "moniker": "tac-node"
        },
        "sync_info": {
            "latest_block_height": "5123456",
            "latest_block_time": "2026-06-01T10:31:22Z",
            "catching_up": false
        },
        "validator_info": {
            "address": "8BFAB70E...",
            "voting_power": "0"
        }
    }
}
```

## Response Fields

| Field           | Type   | Description                                         |
| --------------- | ------ | --------------------------------------------------- |
| node\_info      | object | Node id, network (chain id), and software version   |
| sync\_info      | object | Latest block height/hash/time and catching\_up flag |
| validator\_info | object | This node's validator address and voting power      |

## Use Cases

* **Sync tracking**: Read latest\_block\_height and catching\_up
* **Chain identity**: Confirm node\_info.network before broadcasting

## Error Handling

| Error                     | Message         | Description                                       |
| ------------------------- | --------------- | ------------------------------------------------- |
| -32600 / Invalid request  | Invalid request | The JSON-RPC envelope was malformed               |
| -32602 / Invalid params   | Invalid params  | A parameter was missing or of the wrong type      |
| 403 / RBAC: access denied | Access denied   | The GetBlock access token is missing or incorrect |
