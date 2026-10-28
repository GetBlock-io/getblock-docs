---
description: >-
  Example code for the status JSON-RPC method. Complete guide on how to use
  status JSON-RPC in GetBlock Web3 documentation.
---

# status - NEAR Protocol

Returns the node's general status: sync status, nearcore and protocol version, latest block hash and height, and the current validator set.

## Method

```
status
```

## Parameters

This method takes an empty params array `[]`.

## Example

{% code overflow="wrap" %}
```bash
export NEAR_RPC=https://shared.eu-central-1.getblock.io/<ACCESS-TOKEN>/

curl -X POST "${NEAR_RPC}" \
  -H "Content-Type: application/json" \
  -d '{"jsonrpc": "2.0", "id": "getblock.io", "method": "status", "params": []}'
```
{% endcode %}

## Response

```json
{
    "version": {
        "version": "2.14.0",
        "build": "..."
    },
    "chain_id": "mainnet",
    "latest_protocol_version": 78,
    "sync_info": {
        "latest_block_hash": "9MzuZ...",
        "latest_block_height": 152000000,
        "latest_block_time": "2026-06-01T10:31:22Z",
        "syncing": false
    },
    "validators": [
        {
            "account_id": "figment.poolv1.near",
            "is_slashed": false
        }
    ]
}
```

## Response Fields

| Field                     | Type   | Description                                    |
| ------------------------- | ------ | ---------------------------------------------- |
| chain\_id                 | string | Network identifier (mainnet or testnet)        |
| latest\_protocol\_version | number | Active protocol version                        |
| sync\_info                | object | Latest block hash/height/time and syncing flag |
| validators                | array  | Current validator accounts                     |

## Use Cases

* **Health probe**: Confirm the node is synced (sync\_info.syncing = false)
* **Version check**: Read nearcore and protocol version before using version-gated methods

## Error Handling

| Error                     | Message                  | Description                                                                                             |
| ------------------------- | ------------------------ | ------------------------------------------------------------------------------------------------------- |
| -32700 / Parse error      | Parse error              | The JSON request was malformed                                                                          |
| -32601 / Method not found | Method not found         | The method is unknown or disabled on this node version                                                  |
| HANDLER\_ERROR            | Requested data not found | The account, block, chunk, or transaction does not exist or is garbage-collected (use an archival node) |
| 403 / RBAC: access denied | Access denied            | The GetBlock access token is missing or incorrect                                                       |
